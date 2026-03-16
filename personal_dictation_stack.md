# Personal Dictation Stack

Дата: 2026-03-16 UTC
Цель: отделить пользовательские инструменты голосового ввода от backend ASR и зафиксировать практический shortlist для личной диктовки, письма, заметок, IDE и повседневного ввода текста.

## Что именно сравнивается

Это не сравнение backend API и не сравнение инфраструктуры для продукта.

Здесь сравниваются именно end-user инструменты, которые человек или команда могут поставить себе и использовать для:

- диктовки в любые текстовые поля;
- письма в почте и мессенджерах;
- заметок;
- документов;
- IDE / терминала / prompt-heavy workflow;
- accessibility / RSI / carpal tunnel use cases.

В shortlist включены:

- `Wispr Flow`
- `Superwhisper`
- `Aqua Voice`
- `EdgeWhisper`
- `MacWhisper`

## Краткий вывод

Если убрать backend/API и смотреть только на личную диктовку, картина такая:

1. `Wispr Flow` выглядит как самый polished cloud dictation SaaS.
2. `Superwhisper` выглядит как один из самых сильных приватных/local-first вариантов общего назначения.
3. `Aqua Voice` выглядит как сильный выбор для writing-heavy и technical workflows.
4. `EdgeWhisper` выглядит как интересный privacy-first on-device вариант для Mac.
5. `MacWhisper` остаётся очень сильным инструментом, но он исторически сильнее в file transcription; его system-wide dictation есть, но он не так явно "dictation-first", как лидеры выше.

## Быстрый выбор

### Если нужен лучший готовый cloud dictation SaaS

Берите:

- `Wispr Flow`

Почему:

- работает "in any text field";
- ориентирован именно на диктовку;
- есть dictionary, snippets, AI commands;
- есть командные функции;
- есть Mac, Windows, iPhone, Android.

### Если нужен сильный local/offline-ish private-first инструмент

Берите:

- `Superwhisper`

Почему:

- официальный сайт и docs прямо акцентируют local models;
- одна лицензия на все устройства;
- есть free tier;
- есть local и cloud модели;
- ориентирован на "voice to text that works in any app".

### Если нужен лучший инструмент под writing / coding / technical vocabulary

Берите:

- `Aqua Voice`

Почему:

- делает ставку на refined text, custom instructions и dictionary;
- позиционируется как голосовой ввод для real workflows;
- есть отдельный Avalon API, что косвенно говорит о сильной модели и серьёзной продуктовой ставке.

### Если критичны приватность и on-device работа на Mac

Берите:

- `EdgeWhisper`

Почему:

- 100% on-device;
- позиционируется как privacy-first;
- работает в каждом macOS app;
- нет cloud processing;
- заявлена низкая latency на Apple Silicon.

### Если нужен сильный локальный инструмент ещё и для file transcription

Берите:

- `MacWhisper`

Почему:

- очень сильный стек для transcribe/export/subtitles;
- есть system-wide dictation;
- one-time payment;
- всё локально;
- особенно хорош, если кроме диктовки нужны ещё полноценные transcript workflows.

## Comparison Matrix

| Решение | Платформы | Модель использования | Cloud / Local | Free tier | Базовая цена | Сильная сторона | Ограничение |
|---|---|---|---|---|---|---|---|
| `Wispr Flow` | Mac, Windows, iPhone, Android | dictation SaaS | Cloud | да | `$15/mo` monthly или `$12/user/mo` annual | polished UX, team features, broad platform support | нет public API, нужен интернет |
| `Superwhisper` | Mac, Windows, iOS | dictation app | Local + Cloud | да | `$8.49/mo`, `$84.99/yr`, `$249.99 lifetime` | privacy, local models, cross-device license | pricing/model policy надо внимательно проверять перед покупкой |
| `Aqua Voice` | Mac | dictation app | Cloud-heavy product, privacy claims | да | `$8/mo` annual, team `$12/mo` | technical writing, custom instructions, dictionary | пока менее широкая platform coverage |
| `EdgeWhisper` | Mac | on-device dictation app | Local | да | `$7.99/mo` or `$79.99/yr` | privacy-first on-device dictation | только macOS, 13 languages |
| `MacWhisper` | Mac, iPhone/iPad variant | transcription + dictation tool | Local, optional cloud add-ons | да | `€59` one-time Pro | лучший гибрид диктовки и file transcription | менее чисто "dictation-first" positioning |

## Подтверждённые факты по каждому продукту

### Wispr Flow

Подтверждено по официальным страницам:

- работает `in any text field`;
- поддерживает `100+ languages`;
- есть `custom dictionary and snippets`;
- есть `command mode for editing`;
- есть `privacy mode (Zero Data Retention)`;
- есть `HIPAA-ready`;
- free plan:
  - `2,000 words/week` on desktop
  - `1,000 words/week` on iPhone
  - `Unlimited on Android` limited time
- Pro:
  - `$15/user/mo` monthly
  - `$12/user/mo` billed annually

Практический вывод:

- очень сильный cloud dictation SaaS;
- лучший кандидат, если нужен polished инструмент "поставил и говоришь".

### Superwhisper

Подтверждено по официальным страницам:

- `Voice to text that works in any app`
- `Meeting recording and transcription`
- `Support for 100+ Languages`
- `Unlimited use of small AI models` на free
- Pro даёт:
  - `Use your own AI API Keys`
  - `Unlimited use of Cloud and Local AI models`
  - `Translate any language to English`
  - `Transcribe audio and video files`
- pricing:
  - `$8.49/month`
  - `$84.99/year`
  - `$249.99 once`
- docs говорят про одну Pro license на все устройства;
- careers page прямо говорит про local/private focus и "100% on-device".

Практический вывод:

- сильнейший кандидат, если важны локальная работа и приватность;
- особенно логичен для разработчиков, writers и power users.

### Aqua Voice

Подтверждено по официальным страницам:

- позиционируется как `Fast and Accurate Voice Dictation for Mac and Windows`, но pricing page в открытом виде показывает download for Mac;
- утверждает:
  - `correct grammar`
  - `preserve every word while refining your text in real time`
  - `Custom dictionary`
  - `Custom prompting`
  - `49 languages`
  - `Nothing is stored on our servers`
- pricing:
  - free: `1,000 free words`
  - Pro: `$8/month billed annually`
  - Team: `$12/month billed annually`
- есть отдельный `Avalon API` за `$0.39/hour`

Практический вывод:

- продукт выглядит ориентированным на quality of writing, а не только на raw transcription;
- хороший кандидат для людей, которые много пишут и хотят управлять стилем и терминологией.

### EdgeWhisper

Подтверждено по официальным страницам:

- `100% on-device`
- `No cloud processing`
- `Works in every macOS app`
- `macOS 14+ · Apple Silicon`
- `13 languages`
- заявлена `sub-500ms latency`
- free tier есть;
- Direct Pro:
  - `$79.99/year`
  - `$7.99/month option`

Практический вывод:

- интересный вариант для privacy-sensitive Mac workflows;
- особенно если нужно именно on-device решение без облака.

### MacWhisper

Подтверждено по официальным страницам:

- direct download и App Store версии различаются;
- есть `System wide dictation with Whisper to replace Apple's own dictation`;
- всё локально: `All transcription is done on your device, no data leaves your machine`;
- поддерживает `100 different languages`;
- для Pro:
  - one-time payment;
  - `€59` direct Pro license;
  - file transcription, batch transcription, watch folders, subtitles, transcript workflows;
  - optional cloud integrations через свои API keys/assistant features.

Практический вывод:

- отличный локальный power tool;
- особенно силён, если помимо диктовки нужно много работы с файлами, интервью, субтитрами, экспортами и transcript editing.

## Сравнение по реальным сценариям

### 1. Хочу диктовать в Cursor, VS Code, браузер, Slack, Notion

Лучшие варианты:

- `Wispr Flow`
- `Superwhisper`
- `Aqua Voice`

Если нужен polished UX:

- `Wispr Flow`

Если нужен local/private emphasis:

- `Superwhisper`

Если нужен writing/coding-oriented experience:

- `Aqua Voice`

### 2. Хочу Mac-only решение без облака

Лучшие варианты:

- `EdgeWhisper`
- `Superwhisper`
- `MacWhisper`

Если нужен самый явно privacy-first pitch:

- `EdgeWhisper`

Если нужен самый широкий баланс локальности и зрелости:

- `Superwhisper`

Если кроме диктовки нужны транскрибации файлов:

- `MacWhisper`

### 3. Хочу самый дешёвый способ попробовать современную диктовку

Сначала пробовать:

- `Wispr Flow` free/basic
- `Superwhisper` free
- `Aqua Voice` starter
- `EdgeWhisper` free tier
- `MacWhisper Free`

Но если смотреть на paid entry:

- `EdgeWhisper` и `Aqua Voice` выглядят дешевле или сопоставимо;
- `Superwhisper` хорош тем, что есть lifetime;
- `MacWhisper` хорош one-time purchase моделью;
- `Wispr Flow` дороже, но берёт UX и platform coverage.

### 4. Хочу один инструмент и для диктовки, и для транскрибации файлов

Лучшие варианты:

- `MacWhisper`
- `Superwhisper`

Почему:

- оба уже закрывают не только live dictation, но и file transcription use cases.

## Ranking by Priority

### Лучший polished cloud dictation app

1. `Wispr Flow`
2. `Aqua Voice`

### Лучший private/local-first dictation app

1. `Superwhisper`
2. `EdgeWhisper`
3. `MacWhisper`

### Лучший гибрид dictation + transcription

1. `MacWhisper`
2. `Superwhisper`

### Лучший вариант для developer/power-user workflow

1. `Superwhisper`
2. `Aqua Voice`
3. `Wispr Flow`

Это inference по product positioning и набору функций, а не официальный benchmark maintainers.

## Моя практическая рекомендация

Если выбирать не "вообще лучший", а "что первым пробовать":

1. `Wispr Flow`, если нужен самый polished dictation UX на разных устройствах.
2. `Superwhisper`, если важны локальность, приватность и ощущение power tool.
3. `Aqua Voice`, если основной сценарий это writing/coding и хочется тонкой настройки текста.
4. `MacWhisper`, если помимо диктовки нужны полноценные transcript workflows.
5. `EdgeWhisper`, если нужен Mac-only privacy-first on-device вариант.

## Когда что не брать

Не брать `Wispr Flow`, если:

- нужен offline;
- нужен public API;
- нужен self-host;
- важна полная приватность без облака.

Не брать `Superwhisper`, если:

- нужна простейшая модель "купил SaaS и не думаешь о local/cloud nuance".

Не брать `Aqua Voice`, если:

- нужна широкая multi-platform зрелость уже сегодня.

Не брать `EdgeWhisper`, если:

- нужны Windows/iPhone/Android;
- нужны 100+ языков;
- не устраивает Mac-only стек.

Не брать `MacWhisper`, если:

- нужен чисто polished live dictation-first UX без акцента на file transcription.

## Источники

- Wispr Flow pricing: https://wisprflow.ai/pricing
- Wispr Flow docs overview: https://docs.wisprflow.ai/articles/2772472373-what-is-flow
- Wispr Flow plans: https://docs.wisprflow.ai/articles/9559327591-flow-plans-and-what-s-included
- Superwhisper home: https://superwhisper.com/
- Superwhisper Pro docs: https://superwhisper.com/docs/get-started/sw-pro
- Superwhisper careers: https://superwhisper.com/careers
- Aqua Voice home/pricing: https://aquavoice.com/
- Aqua Avalon API: https://aquavoice.com/avalon-api
- EdgeWhisper: https://edgewhisper.com/
- MacWhisper support docs: https://macwhisper.helpscoutdocs.com/article/40-macwhisper-whisper-transcription-difference
- MacWhisper direct page: https://goodsnooze.gumroad.com/l/macwhisper
