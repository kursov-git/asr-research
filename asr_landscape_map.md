# ASR Landscape Map

Дата: 2026-03-16 UTC
Назначение: свести всё исследование в одну карту решений и больше не смешивать три разных класса задач:

1. личная диктовка;
2. backend/API для продукта;
3. open source/self-hosted ASR.

## Главная мысль

Большая часть путаницы в ASR возникает из-за того, что сравнивают инструменты из разных категорий как будто они конкурируют напрямую.

На практике есть три отдельных мира:

### 1. Personal Dictation Tools

Это инструменты для человека:

- диктовка в любые текстовые поля;
- письма;
- заметки;
- IDE;
- мессенджеры;
- accessibility.

Сюда относятся:

- `Wispr Flow`
- `Superwhisper`
- `Aqua Voice`
- `EdgeWhisper`
- `MacWhisper`

### 2. Managed STT APIs

Это компоненты для разработчиков и продуктовых команд:

- принять аудио;
- отправить в API;
- получить transcript;
- встроить в свой backend.

Сюда относятся:

- `Cloudflare Workers AI Whisper`
- `Groq STT`
- `AssemblyAI`
- `Deepgram`
- `OpenAI Transcription API`

### 3. Open Source / Self-Hosted ASR

Это движки и runtime-стек для тех, кто хочет:

- полный контроль;
- offline;
- on-prem;
- свой inference stack;
- vendor independence.

Сюда относятся:

- `faster-whisper`
- `whisper.cpp`
- `sherpa-onnx`
- `FunASR`
- `WeNet`
- `GigaAM`
- training/fine-tuning toolkits вроде `NeMo`, `SpeechBrain`, `ESPnet`

## One-Page Decision Rule

Используйте это правило:

1. Если вы хотите **сами голосом писать текст**, смотрите только на personal dictation tools.
2. Если вы хотите **строить продукт или сервис**, смотрите только на managed STT APIs или open source/self-host.
3. Если вам нужен **контроль, приватность, offline**, смотрите только на open source/self-host.

Если инструмент не попадает в ваш класс задачи, его надо исключать сразу, даже если он хороший.

## Landscape Matrix

| Решение | Категория | Главный use case | API | Offline/self-host | Для конечного пользователя без кода | Для продукта/backend |
|---|---|---|---|---|---|---|
| `Wispr Flow` | Personal dictation | системная диктовка и writing UX | Нет | Нет | Да | Нет |
| `Superwhisper` | Personal dictation | local/private dictation | Нет публичного product API | Частично local | Да | Нет напрямую |
| `Aqua Voice` | Personal dictation | writing/coding dictation | Не как основной consumer scenario | Нет | Да | Ограниченно |
| `EdgeWhisper` | Personal dictation | on-device dictation on Mac | Нет | Да | Да | Нет |
| `MacWhisper` | Personal dictation + local transcription | диктовка + transcription files | Нет как обычный SaaS API | Да | Да | Ограниченно, если только вручную |
| `Cloudflare Workers AI Whisper` | Managed STT API | дешёвый batch STT | Да | Нет | Нет | Да |
| `Groq STT` | Managed STT API | дешёвый upstream для backend | Да | Нет | Нет | Да |
| `AssemblyAI` | Managed STT API | быстрый API pilot | Да | Нет | Нет | Да |
| `Deepgram` | Managed STT API | realtime/voice product | Да | Нет | Нет | Да |
| `OpenAI Transcription API` | Managed STT API | premium fallback / stronger quality layer | Да | Нет | Нет | Да |
| `faster-whisper` | Open source/self-host | свой batch ASR | Через свою обёртку | Да | Нет | Да |
| `whisper.cpp` | Open source/self-host | local/edge inference | Через свою обёртку | Да | Нет | Да |
| `sherpa-onnx` | Open source/self-host | streaming/mobile runtime | Через свою обёртку | Да | Нет | Да |

## Recommended Picks by Goal

### Goal: Лично диктовать текст

Основной shortlist:

- `Wispr Flow`
- `Superwhisper`
- `Aqua Voice`
- `EdgeWhisper`
- `MacWhisper`

Правило выбора:

- самый polished cloud UX -> `Wispr Flow`
- local/private-first -> `Superwhisper`
- writing/coding focus -> `Aqua Voice`
- strict on-device on Mac -> `EdgeWhisper`
- диктовка + file transcription -> `MacWhisper`

### Goal: Сделать свой backend без своего железа

Основной shortlist:

- `Groq STT`
- `Cloudflare Workers AI Whisper`
- `AssemblyAI`
- `Deepgram`

Правило выбора:

- самый дешёвый batch -> `Cloudflare`
- лучший thin-wrapper upstream -> `Groq`
- лучший бесплатный старт -> `AssemblyAI`
- voice/realtime -> `Deepgram`

### Goal: Полный контроль / offline / self-host

Основной shortlist:

- `faster-whisper`
- `whisper.cpp`
- `sherpa-onnx`

Правило выбора:

- server batch transcription -> `faster-whisper`
- local CPU / Apple Silicon / edge -> `whisper.cpp`
- streaming/mobile/browser runtime -> `sherpa-onnx`

## What Not To Compare Directly

Не надо напрямую сравнивать:

- `Wispr Flow` vs `Groq`
- `MacWhisper` vs `Cloudflare`
- `Superwhisper` vs `AssemblyAI`
- `EdgeWhisper` vs `faster-whisper`

Почему:

- это не конкуренты одного уровня;
- у них разные пользователи;
- разные unit economics;
- разные способы интеграции;
- разные требования к инфраструктуре.

Корректное сравнение должно происходить внутри своего класса.

## My Practical Recommendation

Если бы нужно было оставить по одному базовому выбору в каждом классе:

### Personal dictation

- `Wispr Flow`

Если приоритет это удобство, быстрый старт и polished UX.

### Managed STT API

- `Groq STT`

Если нужен свой продуктовый backend без своего железа.

### Open source/self-host

- `faster-whisper`

Если нужен наиболее практичный старт в self-hosted batch transcription.

## Shortcut Version

Используйте короткое правило:

- "Я хочу сам диктовать" -> `Wispr Flow`
- "Я хочу свой сервис без железа" -> `Groq`
- "Я хочу самый дешёвый batch API" -> `Cloudflare`
- "Я хочу полный контроль" -> `faster-whisper` или `whisper.cpp`

## Связанные документы

- [personal_dictation_stack.md](/home/skris/research/asr/personal_dictation_stack.md)
- [wisprflow_vs_asr_options.md](/home/skris/research/asr/wisprflow_vs_asr_options.md)
- [asr_provider_decision_matrix.md](/home/skris/research/asr/asr_provider_decision_matrix.md)
- [asr_managed_services_shortlist_2026-03-16.md](/home/skris/research/asr/asr_managed_services_shortlist_2026-03-16.md)
- [asr_backend_blueprint.md](/home/skris/research/asr/asr_backend_blueprint.md)
- [asr_open_source_survey_2026-03-14.md](/home/skris/research/asr/asr_open_source_survey_2026-03-14.md)
