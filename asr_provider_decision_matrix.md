# ASR Provider Decision Matrix

Дата: 2026-03-16 UTC
Назначение: быстрый выбор провайдера ASR/STT без собственного железа.

## Как читать эту матрицу

Если нужен быстрый выбор без повторного чтения большого обзора, используйте этот документ как практический cheat sheet:

- выбираете свой сценарий;
- берёте рекомендованный основной вариант;
- при необходимости добавляете fallback;
- дальше уже строите свой backend поверх этого выбора.

## Decision Matrix

| Сценарий | Основной выбор | Fallback | Почему |
|---|---|---|---|
| Нужен самый дешёвый batch STT | `Cloudflare Workers AI Whisper` | `Groq whisper-large-v3-turbo` | Самая низкая цена из проверенных managed-вариантов |
| Нужен свой backend/API без своего железа | `Groq whisper-large-v3-turbo` | `Cloudflare Workers AI Whisper` | Очень дешёвый, удобный upstream для thin-wrapper сервиса |
| Нужен MVP без бюджета | `AssemblyAI` | `Cloudflare Workers AI Whisper` | Самый щедрый free-tier для старта |
| Нужен realtime/voice-oriented сценарий | `Deepgram` | `AssemblyAI` | Лучше заточен под voice/streaming use cases |
| Нужен лучший компромисс цена/качество | `Groq whisper-large-v3-turbo` | `OpenAI gpt-4o-mini-transcribe` | Groq дешёвый как baseline, OpenAI годится как улучшенный fallback |
| Нужен более качественный платный fallback | `OpenAI gpt-4o-mini-transcribe` | `gpt-4o-transcribe` | Удобно эскалировать сложные аудио на более дорогую модель |
| Уже всё живёт в Azure | `Azure Speech` | `Groq whisper-large-v3-turbo` | Можно минимизировать platform sprawl |
| Уже всё живёт в Google Cloud | `Google Cloud Speech-to-Text` | `Groq whisper-large-v3-turbo` | Экосистемная интеграция может быть важнее цены |
| Нужен vendor-agnostic backend | свой API + `Groq` | свой API + `OpenAI` | Проще всего строить abstraction layer поверх OpenAI-compatible style |
| Думаете про self-host на арендованном GPU | сначала не делать | `Runpod/Modal + faster-whisper` только после проверки экономики | Managed чаще выгоднее до подтверждённого объёма |

## Рекомендуемые стандартные наборы

### 1. Минимальный и дешёвый production stack

- primary: `Cloudflare Workers AI Whisper`
- fallback: `Groq whisper-large-v3-turbo`
- orchestration: свой backend с очередью и chunking

Когда брать:

- batch transcription;
- субтитры;
- обработка встреч и звонков;
- внутренние сервисы.

### 2. Самый разумный универсальный stack

- primary: `Groq whisper-large-v3-turbo`
- fallback: `OpenAI gpt-4o-mini-transcribe`
- optional free validation stage: `AssemblyAI`

Когда брать:

- нужен свой API;
- нужна предсказуемая интеграция;
- хотите баланс между ценой, простотой и качеством.

### 3. Voice/realtime stack

- primary: `Deepgram`
- fallback: `Groq whisper-large-v3-turbo`
- optional escalation: `OpenAI gpt-4o-mini-transcribe`

Когда брать:

- voice UI;
- near-real-time обработка;
- streaming-пайплайн важнее минимальной цены.

## Простое правило выбора

Используйте это правило по умолчанию:

1. Если нужен самый дешёвый batch, берите `Cloudflare`.
2. Если нужен свой сервис поверх чужого ASR, берите `Groq`.
3. Если нужно быстро стартовать бесплатно, берите `AssemblyAI`.
4. Если нужен платный quality fallback, добавляйте `OpenAI gpt-4o-mini-transcribe`.
5. Не идите в self-hosted GPU, пока объём не доказал экономику.

## Когда пора переходить на self-host

Переход имеет смысл только если одновременно верны условия:

- уже есть стабильный поток аудио;
- у вас хороший batching и загрузка GPU;
- нужен контроль над приватностью или пайплайном;
- managed cost уже заметен на масштабе;
- команда готова поддерживать inference infra.

Если хотя бы два пункта выше не выполняются, оставайтесь на managed ASR.

## Минимальная рекомендуемая архитектура

- API gateway или backend
- storage для исходных файлов
- очередь задач
- chunking длинных файлов
- вызов primary provider
- fallback на второй provider при ошибке или плохом результате
- сохранение transcript, timestamps и статуса
- webhook или polling API для клиента

## Связанные документы

- [asr_open_source_survey_2026-03-14.md](/home/skris/research/asr/asr_open_source_survey_2026-03-14.md)
- [asr_managed_services_shortlist_2026-03-16.md](/home/skris/research/asr/asr_managed_services_shortlist_2026-03-16.md)
- [asr_research_query.md](/home/skris/research/asr/asr_research_query.md)
