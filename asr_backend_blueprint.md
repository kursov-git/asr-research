# ASR Backend Blueprint

Дата: 2026-03-17 UTC
Назначение: индекс backend-документов и карта слоёв для ASR-сервиса без собственного железа.

## Зачем этот файл переписан

Исходный blueprint был полезен как набор идей, но слишком часто думал сразу про `v0.3`:

- fallback routing;
- provider abstraction;
- chunking strategy;
- observability;
- abuse controls;
- production hardening.

Проблема была не в самих идеях, а в том, что они сидели в одном документе и раздували MVP.

Поэтому backend-часть разделена на три слоя:

1. [mvp_backend_blueprint.md](/home/skris/research/asr/mvp_backend_blueprint.md)
2. [advanced_backend_patterns.md](/home/skris/research/asr/advanced_backend_patterns.md)
3. [production_hardening.md](/home/skris/research/asr/production_hardening.md)

## Как читать теперь

### Если нужно реально начать делать первую версию

Читайте:

- [mvp_backend_blueprint.md](/home/skris/research/asr/mvp_backend_blueprint.md)

Это новый source of truth для первой рабочей версии.

### Если нужно не потерять хорошие, но не первоочередные идеи

Читайте:

- [advanced_backend_patterns.md](/home/skris/research/asr/advanced_backend_patterns.md)

Там вынесены:

- fallback strategy;
- provider abstraction;
- chunking and long-audio strategy;
- richer post-processing;
- extended observability.

### Если нужно думать уже как про production platform

Читайте:

- [production_hardening.md](/home/skris/research/asr/production_hardening.md)

Там вынесены:

- storage hardening;
- queue hardening;
- webhook hardening;
- abuse controls;
- operational reliability;
- error taxonomy and maintenance concerns.

## Новое правило для backend-дизайна

Используйте эту последовательность:

1. `v0.1` = работает
2. `v0.2` = выдерживает нагрузку
3. `v0.3` = переживает реальный прод

Если идея не помогает сделать `v0.1`, она не должна сидеть в основном MVP blueprint.

## Current Recommendation

Для нового сервиса ориентируйтесь так:

- основа: [mvp_backend_blueprint.md](/home/skris/research/asr/mvp_backend_blueprint.md)
- next layer: [advanced_backend_patterns.md](/home/skris/research/asr/advanced_backend_patterns.md)
- later layer: [production_hardening.md](/home/skris/research/asr/production_hardening.md)

## Связанные документы

- [asr_provider_decision_matrix.md](/home/skris/research/asr/asr_provider_decision_matrix.md)
- [asr_managed_services_shortlist_2026-03-16.md](/home/skris/research/asr/asr_managed_services_shortlist_2026-03-16.md)
- [TASKS.md](/home/skris/research/asr/TASKS.md)
