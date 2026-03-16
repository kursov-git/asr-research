# ASR Backend Blueprint

Дата: 2026-03-16 UTC
Назначение: практический шаблон backend-сервиса для транскрибации речи в текст без собственного железа, с managed ASR upstream и fallback-логикой.

## Цель

Построить свой продуктовый ASR-сервис, не обучая модель и не поднимая свой inference cluster.

Под "свой сервис" здесь понимается:

- свой API;
- своё хранилище и статусы задач;
- свой пайплайн обработки;
- свои правила chunking, retry и fallback;
- возможность заменить upstream-провайдера без ломки клиентского API.

## Базовый выбор

Рекомендуемый стартовый стек:

- primary provider: `Groq whisper-large-v3-turbo`
- fallback provider: `OpenAI gpt-4o-mini-transcribe`
- ultra-cheap batch alternative: `Cloudflare Workers AI Whisper`

Если критична минимальная цена:

- primary: `Cloudflare Workers AI Whisper`
- fallback: `Groq whisper-large-v3-turbo`

## High-Level Architecture

Компоненты:

1. `API service`
2. `Object storage`
3. `Queue`
4. `Transcription worker`
5. `Provider adapter layer`
6. `Post-processing layer`
7. `Webhook dispatcher`
8. `Metadata database`

Упрощённая схема:

```text
Client
  -> API
  -> Storage
  -> DB
  -> Queue
  -> Worker
  -> Provider Adapter
  -> ASR Provider
  -> Post-processing
  -> DB
  -> Webhook / Polling API
```

## Основной flow

### 1. Upload

Клиент:

- либо загружает файл напрямую в API;
- либо получает signed upload URL и льёт файл сразу в object storage.

Рекомендация:

- для коротких файлов допустим прямой upload через API;
- для production лучше signed URL, чтобы не гонять большие файлы через backend.

### 2. Create job

После успешной загрузки создаётся запись `transcription_job`:

- `id`
- `status = queued`
- `input_url`
- `input_duration_sec`
- `language_hint`
- `callback_url`
- `provider_strategy`
- `created_at`

Затем job кладётся в очередь.

### 3. Pre-processing

Worker забирает задачу и делает:

- probe медиафайла;
- проверку формата;
- расчёт длительности;
- опциональную нормализацию в `wav 16k mono`;
- chunking длинных файлов;
- опциональный VAD.

### 4. Provider call

Worker выбирает primary provider по policy:

- дешёвый batch -> `Cloudflare`
- универсальный default -> `Groq`
- premium fallback -> `OpenAI`

Если файл длинный или превышает лимит провайдера:

- режется на chunk'и;
- chunk'и отправляются по отдельности;
- затем происходит merge transcript + timestamps.

### 5. Post-processing

После получения сырого результата:

- нормализация текста;
- merge chunk'ов;
- correction of timestamp offsets;
- optional punctuation normalization;
- optional confidence heuristics;
- optional language consistency checks.

### 6. Complete

Результат сохраняется в БД и/или storage:

- `status = completed`
- `text`
- `segments`
- `words`
- `provider_used`
- `processing_time_ms`
- `cost_estimate`

Если был указан callback:

- шлётся webhook.

Если callback нет:

- клиент забирает результат через polling API.

## Fallback Strategy

Не надо сразу бить во все дорогие провайдеры. Делайте ступенчатую эскалацию.

Рекомендуемая стратегия:

1. Primary provider
2. Retry на том же provider при временной ошибке
3. Fallback provider при hard failure
4. Premium fallback только для сложных случаев

### Когда эскалировать на fallback

- upstream timeout;
- provider 5xx;
- пустой transcript;
- transcript подозрительно короткий относительно аудио;
- провал language check;
- low-confidence heuristic;
- превышение лимита размера файла у primary provider.

### Практический policy

- primary: `Groq`
- fallback-1: `Cloudflare` или наоборот, если цель сверхнизкая цена
- fallback-2: `OpenAI gpt-4o-mini-transcribe`

## API Design

### 1. Create transcription job

`POST /v1/transcriptions`

Request:

```json
{
  "file_url": "https://storage.example.com/inbox/file.wav",
  "language": "ru",
  "callback_url": "https://client.example.com/webhooks/transcriptions",
  "timestamps": "word",
  "diarization": false,
  "priority": "normal"
}
```

Response:

```json
{
  "id": "tr_01HXYZ...",
  "status": "queued"
}
```

### 2. Get job status

`GET /v1/transcriptions/{id}`

Response:

```json
{
  "id": "tr_01HXYZ...",
  "status": "processing",
  "provider": "groq",
  "created_at": "2026-03-16T07:00:00Z"
}
```

### 3. Get result

`GET /v1/transcriptions/{id}/result`

Response:

```json
{
  "id": "tr_01HXYZ...",
  "status": "completed",
  "text": "Пример результата транскрибации.",
  "language": "ru",
  "provider_used": "groq",
  "segments": [
    {
      "start": 0.12,
      "end": 2.84,
      "text": "Пример результата транскрибации."
    }
  ],
  "words": [
    {
      "start": 0.12,
      "end": 0.44,
      "word": "Пример"
    }
  ]
}
```

### 4. Webhook payload

```json
{
  "event": "transcription.completed",
  "id": "tr_01HXYZ...",
  "status": "completed",
  "provider_used": "groq",
  "result_url": "https://api.example.com/v1/transcriptions/tr_01HXYZ.../result"
}
```

## Data Model

Минимальный набор таблиц:

### `transcription_jobs`

- `id`
- `status`
- `source_type`
- `source_url`
- `file_name`
- `mime_type`
- `duration_sec`
- `language_hint`
- `callback_url`
- `requested_timestamps`
- `requested_diarization`
- `provider_strategy`
- `provider_used`
- `fallback_count`
- `error_code`
- `error_message`
- `created_at`
- `updated_at`
- `completed_at`

### `transcription_results`

- `job_id`
- `text`
- `detected_language`
- `segments_json`
- `words_json`
- `provider_response_json`
- `cost_estimate_usd`
- `processing_time_ms`

### `transcription_attempts`

- `id`
- `job_id`
- `provider`
- `attempt_number`
- `status`
- `request_size_bytes`
- `response_time_ms`
- `error_code`
- `error_message`
- `created_at`

## Queue Design

Подойдут:

- Redis + BullMQ
- SQS
- RabbitMQ
- Postgres-backed queue для very small scale

Рекомендация:

- для быстрого старта: `Redis + BullMQ` или `RQ/Celery`
- для cloud-native production: `SQS`

Нужны:

- retry policy;
- dead-letter queue;
- visibility timeout;
- job deduplication, если клиент ретраит create запрос.

## Chunking Strategy

Нельзя рассчитывать, что любой upstream съест любой файл целиком.

Поэтому chunking должен быть частью сервиса с первого дня.

Рекомендуемый подход:

1. Если файл короткий и влезает в provider limits -> отправлять целиком.
2. Если длинный:
   - нормализовать аудио;
   - резать по VAD или fixed windows;
   - добавлять overlap;
   - собирать назад с offset correction.

Рекомендуемые стартовые параметры:

- chunk length: `5-15 min` для batch
- overlap: `1-3 sec`
- для noisy audio лучше VAD-based segmentation

## Provider Adapter Layer

Никогда не завязывайте бизнес-логику напрямую на response format конкретного провайдера.

Сделайте унифицированный интерфейс:

```ts
interface AsrProvider {
  transcribe(input: TranscribeInput): Promise<TranscribeOutput>;
}
```

Где `TranscribeOutput` унифицирован:

- `text`
- `segments`
- `words`
- `language`
- `raw`
- `provider`

Это позволит:

- менять провайдеров без переписывания worker logic;
- включать fallback;
- сравнивать провайдеров на одном формате ответа.

## Post-Processing Rules

Минимальный набор правил:

- trim and whitespace normalization;
- merge duplicated overlap text;
- fix timestamp offsets after chunk merge;
- normalize punctuation spacing;
- preserve raw provider payload отдельно.

Что не стоит делать слишком рано:

- агрессивную правку текста LLM'ом;
- умную "коррекцию смысла";
- автоисправление имён без словаря.

Сначала нужна стабильная воспроизводимость.

## Observability

Нужно логировать и метрики, и трассировку попыток.

Минимальные метрики:

- jobs created
- jobs completed
- jobs failed
- processing latency
- upstream latency by provider
- fallback rate
- average transcript length
- estimated cost by provider
- empty transcript count

Минимальные алерты:

- spike in failures
- spike in empty transcripts
- provider latency degradation
- provider-specific error burst

## Security and Abuse Controls

Минимально обязательно:

- signed upload URLs
- max file size
- allowed mime types
- auth on API
- rate limiting
- callback URL allowlist or signature verification
- storage lifecycle policy

Если сервис внешний:

- защита от заливки произвольных огромных файлов;
- tenant quotas;
- per-tenant billing or usage accounting.

## MVP Scope

Что должно войти в MVP:

- upload by URL or direct upload
- async transcription job
- polling status endpoint
- result endpoint
- one primary provider
- one fallback provider
- chunking for long audio
- timestamps support
- basic metrics

Что можно отложить:

- diarization
- speaker labels
- custom vocabulary UI
- billing
- multi-tenant quota dashboards
- transcript editing UI

## Recommended Tech Choices

### Backend

- `FastAPI` если нужен быстрый Python MVP
- `NestJS` если команда сильнее в TypeScript

### Queue

- `BullMQ` для Node stack
- `Celery` или `RQ` для Python stack

### Storage

- `S3`-compatible object storage
- `Cloudflare R2` если уже идёте в Cloudflare

### DB

- `Postgres`

### Media tools

- `ffmpeg`
- `ffprobe`

## Deployment Phases

### Phase 1. Thin-wrapper MVP

- один provider
- async jobs
- polling
- без diarization

Цель:

- доказать ценность продукта и получить реальные аудиоданные.

### Phase 2. Production baseline

- primary + fallback
- chunking
- webhook
- metrics
- retries and dead-letter queue

### Phase 3. Optimization

- provider routing by scenario
- premium fallback for hard files
- VAD improvements
- language-specific rules
- cost optimization

## Recommended Default Plan

Если нужно начать без лишних решений:

1. Backend: `FastAPI`
2. Queue: `Redis + RQ` или `BullMQ`, если TS stack
3. Storage: `S3-compatible`
4. Primary: `Groq whisper-large-v3-turbo`
5. Fallback: `OpenAI gpt-4o-mini-transcribe`
6. Long files: chunking with overlap
7. Client API: async jobs + polling + webhook

Это даст:

- быстрое время до первой рабочей версии;
- низкую стоимость;
- нормальный путь к production без смены контракта API.

## Связанные документы

- [asr_provider_decision_matrix.md](/home/skris/research/asr/asr_provider_decision_matrix.md)
- [asr_managed_services_shortlist_2026-03-16.md](/home/skris/research/asr/asr_managed_services_shortlist_2026-03-16.md)
- [asr_open_source_survey_2026-03-14.md](/home/skris/research/asr/asr_open_source_survey_2026-03-14.md)
