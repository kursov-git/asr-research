# ASR без своего железа: free сервисы, дешёвые managed API и shortlist

Дата: 2026-03-16 UTC
Цель: понять, что брать, если собственного железа для ASR нет, и что выгоднее: готовый облачный STT API или свой сервис поверх дешёвого AI/арендованных GPU.

## Исходный подробный запрос для продолжения исследования

Найти и сравнить актуальные варианты speech-to-text, если у команды нет собственного железа для инференса.

Нужно:

1. Найти бесплатные или почти бесплатные managed STT/ASR сервисы.
2. Найти дешёвые production-ready API для транскрибации речи в текст.
3. Отдельно оценить вариант "сделать свой сервис", но не обучать свою модель:
   - взять существующую open source модель;
   - запускать её на дешёвой аренде GPU или serverless GPU;
   - завернуть в свой API/backend.
4. Для каждого варианта проверить:
   - бесплатный лимит;
   - list pricing;
   - batch vs streaming;
   - file size / practical limits;
   - timestamps / diarization / add-ons;
   - пригодность для MVP;
   - пригодность для production;
   - скрытые эксплуатационные риски.
5. На выходе дать shortlist:
   - самый дешёвый;
   - самый простой;
   - лучший по качеству/цене;
   - лучший для собственного thin-wrapper сервиса;
   - когда уже имеет смысл арендовать GPU и крутить open source самим.

Использовать только официальные источники:

- pricing pages;
- официальные docs;
- официальные model cards;
- официальные GitHub README и benchmark tables.

Если делаются расчёты:

- явно помечать их как расчёт/inference;
- приводить формулу;
- использовать абсолютные даты.

## Охват поиска

В этом проходе были проверены:

- Cloudflare Workers AI
- Groq Speech-to-Text
- OpenAI Transcription API
- AssemblyAI
- Deepgram
- Azure Speech
- Google Cloud Speech-to-Text
- Modal
- Runpod
- `faster-whisper` benchmark как ориентир для self-hosted economics

## Краткий вывод

Если своего железа нет, то в большинстве случаев выгоднее не "поднимать свой ASR", а сделать свой API/сервис поверх managed STT.

Практический ranking:

1. Самый дешёвый production batch STT: `Cloudflare Workers AI Whisper`
2. Самый практичный дешёвый API для своего backend: `Groq whisper-large-v3-turbo`
3. Самый щедрый free-tier для старта без бюджета: `AssemblyAI`
4. Лучший вариант, если готовы платить заметно больше за качество текста: `OpenAI gpt-4o-mini-transcribe`

Свой self-hosted inference на дешёвом GPU имеет смысл только если:

- есть стабильный поток часов аудио;
- вы умеете держать GPU хорошо загруженным;
- нужен контроль, приватность или кастомная обработка;
- и вы готовы поддерживать inference stack.

Для малого и среднего объёма managed API почти всегда проще и экономически лучше.

## Результаты поиска

### 1. Cloudflare Workers AI

Что нашлось:

- Workers AI включён и в `Free`, и в `Paid` планы.
- Официальная pricing page говорит про `10,000 Neurons per day` бесплатно.
- Для `@cf/openai/whisper` цена указана как `$0.0005 per audio minute`, для `@cf/openai/whisper-large-v3-turbo` тоже `$0.0005 per audio minute`.

Факты из официальных docs:

- free allocation: `10,000 Neurons per day`
- pricing above free tier: `$0.011 / 1,000 Neurons`
- `@cf/openai/whisper`: `41.14 neurons per audio minute`
- `@cf/openai/whisper-large-v3-turbo`: `46.63 neurons per audio minute`

Расчёт:

- `10,000 / 46.63 = 214.45` минут в день бесплатно для `whisper-large-v3-turbo`
- это примерно `3.57` часа в день
- если ежедневно выбирать весь free limit, это около `107` часов в 30 дней

Оценка:

- это самый дешёвый managed batch-вариант из проверенных;
- очень хорош для MVP, внутренних сервисов, сабов, back-office транскрибации;
- сильнее всего выигрывает там, где не нужен сложный realtime voice agent stack.

Ограничение:

- Cloudflare хорош как дешёвый STT-бэкенд, но это не самый удобный выбор для сложного realtime voice-продукта.

### 2. Groq Speech-to-Text

Что нашлось:

- Groq docs подтверждают OpenAI-compatible speech-to-text endpoint.
- В docs указаны `segment` и `word` timestamps.
- Официальный pricing page указывает:
  - `whisper-large-v3`: `$0.111 per hour`
  - `whisper-large-v3-turbo`: `$0.04 per hour`
- В speech-to-text docs указано ограничение:
  - `25 MB` для `free tier`
  - `100 MB` для `dev tier`

Оценка:

- это один из лучших вариантов, если нужен свой backend без своего железа;
- простая интеграция;
- цена очень низкая;
- хорошие практические возможности вроде word timestamps;
- особенно хорош как thin-wrapper service: ваш API принимает файл и проксирует его в Groq.

Ограничения:

- не видно щедрого free-volume, как у AssemblyAI;
- file-size limit на free/dev tier надо учитывать;
- для really long audio всё равно нужен chunking.

### 3. OpenAI Transcription API

Что нашлось на pricing page:

- `gpt-4o-mini-transcribe`: `$0.003 / minute`
- `gpt-4o-transcribe`: `$0.006 / minute`
- `Whisper`: `$0.006 / minute`

В пересчёте на час:

- `gpt-4o-mini-transcribe`: `$0.18 / hour`
- `gpt-4o-transcribe`: `$0.36 / hour`
- `Whisper`: `$0.36 / hour`

Оценка:

- существенно дороже Cloudflare и Groq;
- но всё ещё недорого в абсолютных цифрах, если объёмы не огромные;
- хороший кандидат как "более качественный fallback" для сложных аудио.

### 4. AssemblyAI

Что нашлось на официальной pricing page:

- free:
  - `185 hours` pre-recorded audio
  - `333 hours` streaming audio
  - `up to 5 new streams per minute`
- pay as you go:
  - `as low as $0.15/hr`

Оценка:

- самый щедрый бесплатный старт среди просмотренных вариантов;
- отличный вариант, если нужно быстро проверить продукт и не тратить деньги;
- подходит скорее как пилотный/прототипный managed provider, чем как абсолютный лидер по минимальной цене.

### 5. Deepgram

Что нашлось на официальной pricing page:

- free: `$200 of credit`, без карты, pay-as-you-go после этого
- `Flux`: `$0.0077/min`
- `Nova-3 (Monolingual)`: `$0.0077/min`
- `Nova-3 (Multilingual)`: `$0.0092/min`
- `Nova-1 & 2`: `$0.0058/min`
- add-ons:
  - speaker diarization: `$0.0020/min`
  - keyterm prompting: `$0.0013/min`

В пересчёте:

- Nova-3 mono: `0.0077 * 60 = $0.462/hour`
- Nova-3 multilingual: `0.0092 * 60 = $0.552/hour`

Оценка:

- хороший voice/streaming provider;
- заметно дороже Groq и Cloudflare;
- имеет смысл, если вам нужны его voice-specific возможности, streaming UX или ecosystem fit.

### 6. Azure Speech

На официальной pricing page подтверждено:

- `Free (F0)`
- `Standard Real-time Transcription: 5 audio hours free per month`
- `Custom Real-time Transcription: 5 audio hours free per month`
- примечание: free hours shared between Standard and Custom

Оценка:

- удобно, если вы уже в Azure;
- как самостоятельный cheapest-path не выглядит лучшим выбором.

### 7. Google Cloud Speech-to-Text

На официальных страницах подтверждено:

- `Speech-to-Text V2`: `$0.016 per min`
- `All users can send up to 60 minutes of audio ... for free each month`
- `New customers get up to $300 in free credits`

В пересчёте:

- `$0.016/min = $0.96/hour`

Оценка:

- дороговато для default choice;
- имеет смысл, если уже есть сильная зависимость от Google Cloud.

## Сводная таблица цен и free-tier

Все цены ниже checked on 2026-03-16 UTC. Это list prices, без хранения, трафика, add-ons и volume discounts.

| Провайдер | Free-tier | Цена | Цена за 1 час аудио | Комментарий |
|---|---|---:|---:|---|
| Cloudflare Workers AI Whisper | 10,000 neurons/day | $0.0005/min | $0.03/hr | Самый дешёвый managed batch |
| Groq whisper-large-v3-turbo | free tier есть, но в docs здесь виден только file-size limit | $0.04/hr | $0.04/hr | Лучший дешёвый API для своего backend |
| AssemblyAI | 185h pre-recorded + 333h streaming | starting at $0.15/hr | $0.15/hr | Лучший бесплатный старт |
| OpenAI gpt-4o-mini-transcribe | явного STT free-tier на pricing page не видно | $0.003/min | $0.18/hr | Качество/простота, но дороже |
| Deepgram Nova-3 mono | $200 credits | $0.0077/min | $0.462/hr | Хорошо для speech stacks |
| Google STT V2 | 60 min/mo + up to $300 new customer credits | $0.016/min | $0.96/hr | Дорого для default choice |
| Azure Speech | 5h/mo | региональный pricing | n/a in this pass | Скорее экосистемный выбор |

## Cost table для 100 / 1000 / 10000 часов

Расчёт по list price, без add-ons.

| Вариант | 100 часов | 1000 часов | 10000 часов |
|---|---:|---:|---:|
| Cloudflare Workers AI Whisper | $3 | $30 | $300 |
| Groq whisper-large-v3-turbo | $4 | $40 | $400 |
| AssemblyAI starting at | $15 | $150 | $1500 |
| OpenAI gpt-4o-mini-transcribe | $18 | $180 | $1800 |
| Deepgram Nova-3 mono | $46.20 | $462 | $4620 |
| Google STT V2 | $96 | $960 | $9600 |

## Шортлист

### 1. Самый дешёвый production-вариант

Выбор: `Cloudflare Workers AI Whisper`

Почему:

- наименьшая list price среди проверенных managed вариантов;
- serverless;
- очень сильный бесплатный daily quota;
- хорошо подходит для batch transcription и внутреннего API.

Когда не брать:

- если нужен богатый realtime voice stack;
- если критичен вендор-agnostic OpenAI-like API experience с минимумом ограничений.

### 2. Самый практичный вариант для собственного thin-wrapper сервиса

Выбор: `Groq whisper-large-v3-turbo`

Почему:

- `$0.04/hour` всё ещё очень дёшево;
- OpenAI-compatible endpoint;
- word/segment timestamps;
- хорош как upstream для вашего собственного backend.

Рекомендуемая архитектура:

- `FastAPI` или `NestJS`
- storage для исходных файлов
- очередь задач
- chunking больших файлов
- отправка в Groq
- сохранение transcript + timestamps
- webhook/polling API для клиента

### 3. Лучший бесплатный старт

Выбор: `AssemblyAI`

Почему:

- очень крупный free-tier;
- можно быстро протестировать продукт без бюджета;
- хорошо подходит для discovery stage.

Когда не брать:

- если вы уже знаете, что пойдёте в production и стоимость на объёмах критична.

### 4. Лучший "качественнее, но ещё разумно по цене"

Выбор: `OpenAI gpt-4o-mini-transcribe`

Почему:

- цена не минимальная, но всё ещё умеренная;
- удобен как платный fallback-провайдер;
- логично использовать для "сложных файлов", если baseline upstream дал плохой результат.

## Делать ли свой сервис без своего железа

Да, но в смысле:

- не своя модель;
- не свой training pipeline;
- а свой продуктовый слой поверх чужого ASR.

Рекомендованный вариант:

1. Свой API принимает аудио.
2. Делает валидацию, chunking, VAD, retry logic.
3. Отправляет в дешёвый STT upstream.
4. Хранит transcript, timestamps, метаданные и статус.
5. При ошибке или плохом confidence перевызывает другой провайдер.

Практически это даёт:

- контроль над продуктом;
- возможность менять провайдера без ломки клиента;
- отсутствие GPU-эксплуатации.

## Когда имеет смысл арендовать GPU и крутить open source самому

### Данные по дешёвым serverless GPU

Проверенные цены:

- Modal:
  - `T4`: `$0.000164/sec` = примерно `$0.5904/hour`
  - `L4`: `$0.000222/sec` = примерно `$0.7992/hour`
  - `A10`: `$0.000306/sec` = примерно `$1.1016/hour`
  - Starter: `$30/month free credit`
- Runpod serverless:
  - `L4, A5000, 3090`: `active $0.00013/sec` = примерно `$0.468/hour`
  - `A40, A6000`: `active $0.00024/sec` = примерно `$0.864/hour`

### Break-even against managed STT

Формула:

- `required realtime factor = GPU hourly cost / ASR cost per audio hour`

Это значит, сколько часов аудио в час GPU должен стабильно переваривать, чтобы только по compute выйти в ноль относительно managed API.

#### Против Groq (`$0.04/hour audio`)

| GPU | Стоимость GPU/час | Нужный throughput для break-even |
|---|---:|---:|
| Modal T4 | $0.5904 | 14.76x real-time |
| Modal L4 | $0.7992 | 19.98x real-time |
| Modal A10 | $1.1016 | 27.54x real-time |
| Runpod L4/A5000/3090 active | $0.468 | 11.70x real-time |
| Runpod A40/A6000 active | $0.864 | 21.60x real-time |

#### Против Cloudflare Whisper (`$0.03/hour audio`)

| GPU | Стоимость GPU/час | Нужный throughput для break-even |
|---|---:|---:|
| Modal T4 | $0.5904 | 19.68x real-time |
| Modal L4 | $0.7992 | 26.64x real-time |
| Modal A10 | $1.1016 | 36.72x real-time |
| Runpod L4/A5000/3090 active | $0.468 | 15.60x real-time |
| Runpod A40/A6000 active | $0.864 | 28.80x real-time |

### Почему это важно

`faster-whisper` в официальном README показывает для `large-v2` на GPU benchmark:

- `13 minutes` аудио за `1m03s` на `RTX 3070 Ti`
- это около `12.4x real-time`
- при `batch_size=8`: `17s`, то есть примерно `45.9x real-time`, но это уже batch-сценарий с более агрессивной упаковкой нагрузки

Вывод из этого:

- для realtime или обычных мелких задач self-host на дешёвом GPU часто не побеждает `Groq`, а почти никогда не побеждает `Cloudflare`, если считать только compute;
- self-host может начать выигрывать только при хорошо загруженном batch processing, с грамотной упаковкой задач и без больших idle periods;
- как только добавить devops, хранение, мониторинг, cold starts, retries и обновление моделей, managed чаще снова выглядит выгоднее.

## Финальная рекомендация

Если нужно принять решение сейчас:

1. Для самого дешёвого production batch STT берите `Cloudflare Workers AI Whisper`.
2. Если нужен свой backend/API, но без своего железа, берите `Groq whisper-large-v3-turbo`.
3. Если цель сейчас только быстро проверить идею без бюджета, берите `AssemblyAI`.
4. Если хотите держать более качественный или более "премиальный" fallback, добавьте `OpenAI gpt-4o-mini-transcribe`.
5. Не арендуйте GPU сразу, если нет устойчивого потока задач и не подтверждённой экономики.

## Источники

- Cloudflare Workers AI pricing: https://developers.cloudflare.com/workers-ai/platform/pricing/
- Cloudflare whisper-large-v3-turbo model page: https://developers.cloudflare.com/workers-ai/models/whisper-large-v3-turbo/
- Groq speech-to-text docs: https://console.groq.com/docs/speech-to-text
- Groq pricing: https://groq.com/pricing
- Groq whisper-large-v3-turbo model page: https://console.groq.com/docs/model/whisper-large-v3-turbo
- OpenAI pricing: https://developers.openai.com/api/docs/pricing
- AssemblyAI pricing: https://www.assemblyai.com/pricing
- Deepgram pricing: https://deepgram.com/pricing
- Azure Speech pricing: https://azure.microsoft.com/en-us/pricing/details/speech/
- Google Cloud Speech-to-Text: https://cloud.google.com/speech-to-text
- Google Cloud STT requests docs: https://docs.cloud.google.com/speech-to-text/docs/v1/speech-to-text-requests
- Modal pricing: https://modal.com/pricing
- Runpod serverless pricing: https://docs.runpod.io/serverless/pricing
- faster-whisper README/benchmark: https://github.com/SYSTRAN/faster-whisper
