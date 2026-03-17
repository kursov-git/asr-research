# ASR Research

Дата: 2026-03-16 UTC
Назначение: оглавление репозитория `/home/skris/research/asr` и быстрый маршрут по материалам исследования.

## Что это за репозиторий

Это curated knowledge base по speech-to-text решениям.

Репозиторий покрывает три отдельные области:

1. personal dictation tools
2. managed STT APIs
3. open source / self-hosted ASR

Основное правило репозитория:

- не смешивать эти три класса решений в одну матрицу

## Что лежит в этой папке

Эта папка содержит исследование по трём разным, но связанным темам:

1. Open source ASR и self-hosted speech-to-text
2. Managed STT/API провайдеры без собственного железа
3. Personal dictation tools для пользователя и команды

Главная цель папки:

- не смешивать разные классы решений;
- быстро выбирать правильный документ под конкретную задачу;
- сохранить контекст для продолжения исследования другими агентами или людьми.

## Repo Meta

- License: [LICENSE](/home/skris/research/asr/LICENSE)
- Contribution guide: [CONTRIBUTING.md](/home/skris/research/asr/CONTRIBUTING.md)
- Machine-readable catalog: [catalog.json](/home/skris/research/asr/catalog.json)
- CSV index: [index.csv](/home/skris/research/asr/index.csv)

## С чего читать

Если времени мало, начните с этого порядка:

1. [asr_landscape_map.md](/home/skris/research/asr/asr_landscape_map.md)
2. [asr_provider_decision_matrix.md](/home/skris/research/asr/asr_provider_decision_matrix.md)
3. нужный специализированный документ по вашему сценарию

## Документы по назначению

### 1. Общая карта решений

- [asr_landscape_map.md](/home/skris/research/asr/asr_landscape_map.md)

Когда читать:

- если нужно быстро понять весь ландшафт;
- если нужно не смешивать dictation, API и self-host;
- если нужен one-page rule выбора.

### 2. Open source / self-host ASR

- [asr_open_source_survey_2026-03-14.md](/home/skris/research/asr/asr_open_source_survey_2026-03-14.md)
- [asr_research_query.md](/home/skris/research/asr/asr_research_query.md)

Когда читать:

- если нужно решить, делать ли свой ASR;
- если нужен shortlist по open source движкам;
- если нужна база для дальнейшего исследования.

### 3. Managed STT APIs и сервис без своего железа

- [asr_managed_services_shortlist_2026-03-16.md](/home/skris/research/asr/asr_managed_services_shortlist_2026-03-16.md)
- [asr_provider_decision_matrix.md](/home/skris/research/asr/asr_provider_decision_matrix.md)
- [asr_backend_blueprint.md](/home/skris/research/asr/asr_backend_blueprint.md)
- [mvp_backend_blueprint.md](/home/skris/research/asr/mvp_backend_blueprint.md)
- [advanced_backend_patterns.md](/home/skris/research/asr/advanced_backend_patterns.md)
- [production_hardening.md](/home/skris/research/asr/production_hardening.md)

Когда читать:

- если нужен backend для продукта;
- если нет своего железа;
- если нужно выбрать между `Groq`, `Cloudflare`, `AssemblyAI`, `Deepgram` и self-host.
- если нужно понять, что должно войти в MVP, а что нужно вынести в later stages.

### 4. Personal dictation / productivity tools

- [personal_dictation_stack.md](/home/skris/research/asr/personal_dictation_stack.md)
- [wisprflow_vs_asr_options.md](/home/skris/research/asr/wisprflow_vs_asr_options.md)
- [FUTURE_PROJECTS.md](/home/skris/research/asr/FUTURE_PROJECTS.md)

Когда читать:

- если задача это личная диктовка;
- если нужен SaaS-инструмент для письма и ввода текста;
- если нужно отдельно оценить `Wispr Flow`.

## Быстрый выбор по задачам

### Я хочу лично диктовать текст

Читайте:

- [personal_dictation_stack.md](/home/skris/research/asr/personal_dictation_stack.md)

Короткий вывод:

- `Wispr Flow` как polished cloud dictation tool
- `Superwhisper` как strong local/private-first option
- `MacWhisper` если нужна ещё и file transcription

### Я хочу сделать свой backend без железа

Читайте:

- [asr_provider_decision_matrix.md](/home/skris/research/asr/asr_provider_decision_matrix.md)
- [mvp_backend_blueprint.md](/home/skris/research/asr/mvp_backend_blueprint.md)

Короткий вывод:

- `Groq` как default upstream
- `Cloudflare` как cheapest batch option
- `AssemblyAI` как generous free start
- один provider и polling важнее раннего fallback и production-излишеств

### Я хочу self-host / open source / offline

Читайте:

- [asr_open_source_survey_2026-03-14.md](/home/skris/research/asr/asr_open_source_survey_2026-03-14.md)

Короткий вывод:

- `faster-whisper` для практичного batch self-host
- `whisper.cpp` для local/edge
- `sherpa-onnx` для streaming/mobile runtime

## Что не надо смешивать

Некорректные сравнения:

- `Wispr Flow` vs `Groq`
- `Cloudflare` vs `MacWhisper`
- `Superwhisper` vs `AssemblyAI`
- `EdgeWhisper` vs `faster-whisper`

Почему:

- это разные классы решений;
- у них разные пользователи;
- разные unit economics;
- разные способы внедрения.

## Recommended Reading Paths

### Path A. Product/backend

1. [asr_landscape_map.md](/home/skris/research/asr/asr_landscape_map.md)
2. [asr_provider_decision_matrix.md](/home/skris/research/asr/asr_provider_decision_matrix.md)
3. [asr_managed_services_shortlist_2026-03-16.md](/home/skris/research/asr/asr_managed_services_shortlist_2026-03-16.md)
4. [mvp_backend_blueprint.md](/home/skris/research/asr/mvp_backend_blueprint.md)
5. [advanced_backend_patterns.md](/home/skris/research/asr/advanced_backend_patterns.md)

### Path B. Open source/self-host

1. [asr_landscape_map.md](/home/skris/research/asr/asr_landscape_map.md)
2. [asr_open_source_survey_2026-03-14.md](/home/skris/research/asr/asr_open_source_survey_2026-03-14.md)
3. [asr_research_query.md](/home/skris/research/asr/asr_research_query.md)

### Path C. Personal dictation

1. [asr_landscape_map.md](/home/skris/research/asr/asr_landscape_map.md)
2. [personal_dictation_stack.md](/home/skris/research/asr/personal_dictation_stack.md)
3. [wisprflow_vs_asr_options.md](/home/skris/research/asr/wisprflow_vs_asr_options.md)

## Suggested Next Research

- реальный bake-off на собственных аудиоданных;
- матрица `RU/EN`, `batch/streaming`, `desktop/mobile`, `private/cloud`;
- cost model на конкретных monthly volumes;
- внедренческий шаблон сервиса с примерами кода;
- evaluation set и scoring methodology;
- shortlist по русскоязычной диктовке отдельно.

## Parked Ideas

- [FUTURE_PROJECTS.md](/home/skris/research/asr/FUTURE_PROJECTS.md)

Сейчас туда вынесена тема отдельного voice-input/dictation проекта как future vibe-coding candidate.

## Companion Context File

Для других агентов используйте:

- [AGENT_CONTEXT.md](/home/skris/research/asr/AGENT_CONTEXT.md)

Это основной handoff-документ для продолжения работы.
