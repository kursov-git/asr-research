# MVP Backend Blueprint

Дата: 2026-03-17 UTC
Назначение: минимальный backend-план для первой рабочей версии ASR-сервиса без собственного железа.

## Scope

Этот документ описывает только `v0.1`.

Цель:

- принять аудио;
- поставить задачу в очередь;
- отдать результат транскрибации;
- не пытаться решать весь production заранее.

Это не документ про:

- многоуровневый fallback;
- provider routing;
- продвинутый chunking;
- webhook-heavy integrations;
- сложную observability платформу;
- abuse-hardening уровня публичного multi-tenant SaaS.

Для этого есть:

- [advanced_backend_patterns.md](/home/skris/research/asr/advanced_backend_patterns.md)
- [production_hardening.md](/home/skris/research/asr/production_hardening.md)

## MVP Goal

Собрать первую версию, которая:

- стабильно принимает аудио;
- асинхронно транскрибирует его через один provider;
- позволяет клиенту забрать статус и результат;
- не переусложнена до старта.

## Default Stack

Рекомендуемый старт:

- backend: `FastAPI`
- queue: `Redis + RQ` или `Celery`
- storage: `S3-compatible`
- database: `Postgres`
- primary provider: `Groq whisper-large-v3-turbo`

Если критична минимальная стоимость batch:

- primary provider: `Cloudflare Workers AI Whisper`

## MVP Principles

1. Один provider
2. Один async job flow
3. Polling вместо webhook
4. Минимальная schema
5. Базовые лимиты на вход
6. Минимальный post-processing

## Minimal Architecture

Компоненты:

1. `API service`
2. `Object storage`
3. `Queue`
4. `Transcription worker`
5. `Database`

Упрощённая схема:

```text
Client
  -> API
  -> Storage
  -> DB
  -> Queue
  -> Worker
  -> Provider
  -> DB
  -> Polling API
```

## Minimal Flow

### 1. Upload

Два допустимых варианта:

- direct upload через API для простого старта;
- signed upload URL, если файл может быть крупным.

Для первой версии допустимо начать с direct upload, если:

- файл небольшой;
- объём невелик;
- сервис внутренний.

### 2. Create job

После upload создаётся job:

- `id`
- `status = queued`
- `source_url`
- `mime_type`
- `created_at`

Job кладётся в очередь.

### 3. Worker processing

Worker делает только обязательный минимум:

- проверка, что файл читается;
- определение длительности;
- вызов provider;
- сохранение результата;
- перевод статуса в `completed` или `failed`.

Не делать по умолчанию:

- VAD
- chunk overlap merge
- multi-provider routing
- transcript heuristics

### 4. Result retrieval

Клиент опрашивает статус и затем получает результат через polling.

## Minimal API

### Create job

`POST /v1/transcriptions`

Response:

```json
{
  "id": "tr_01HXYZ...",
  "status": "queued"
}
```

### Get job

`GET /v1/transcriptions/{id}`

Response:

```json
{
  "id": "tr_01HXYZ...",
  "status": "processing"
}
```

### Get result

`GET /v1/transcriptions/{id}/result`

Response:

```json
{
  "id": "tr_01HXYZ...",
  "status": "completed",
  "text": "Пример результата",
  "language": "ru"
}
```

## Minimal Data Model

### `transcription_jobs`

- `id`
- `status`
- `source_url`
- `mime_type`
- `duration_sec`
- `provider_used`
- `error_message`
- `created_at`
- `updated_at`

### `transcription_results`

- `job_id`
- `text`
- `language`
- `provider_response_json`

Не добавлять в MVP по умолчанию:

- `fallback_count`
- `provider_strategy`
- `requested_diarization`
- `cost_estimate_usd`
- `attempt_number`
- `transcription_attempts`

## Minimal Security

Обязательный минимум:

- auth на API
- `max file size`
- allowlist MIME types
- request timeout
- basic rate limiting

Этого достаточно для `v0.1`.

Более жёсткие меры перенесены в:

- [production_hardening.md](/home/skris/research/asr/production_hardening.md)

## Minimal Post-Processing

Разрешённый минимум:

- trim
- whitespace normalization

Не делать в MVP по умолчанию:

- punctuation repair
- semantic cleanup
- language validation heuristics
- overlap merge logic

## Minimal Metrics

Нужны только:

- jobs created
- jobs completed
- jobs failed
- processing latency

Этого достаточно для первой версии.

## What To Explicitly Defer

Отложить в `v0.2+`:

- fallback provider
- provider abstraction layer
- chunking
- webhook callbacks
- advanced metrics
- cost tracking
- quota system
- tenant billing

## Definition Of Done For MVP

MVP готов, если:

- файл можно загрузить;
- job создаётся;
- worker стабильно обрабатывает job;
- результат можно получить по polling;
- ошибки не теряются;
- на одном provider всё проходит end-to-end.

## Next Layer

После этого переходите к:

- [advanced_backend_patterns.md](/home/skris/research/asr/advanced_backend_patterns.md)
