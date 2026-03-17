# Production Hardening

Дата: 2026-03-17 UTC
Назначение: сохранить production-level concerns отдельно от MVP и advanced patterns.

## Scope

Это слой `v0.3`.

Его стоит открывать только когда:

- MVP уже жив;
- basic async flow работает стабильно;
- есть реальный трафик или близкая перспектива реального трафика.

## 1. Queue Hardening

Что нужно:

- retry policy
- dead-letter queue
- visibility timeout
- job deduplication
- poison-job handling

Зачем:

- реальный прод не любит silent failures и stuck jobs.

## 2. Upload And Storage Hardening

Что нужно:

- signed upload URLs
- storage lifecycle policy
- orphaned upload cleanup
- protection against oversized uploads
- clear retention policy

Ключевой риск:

- дешёвый abuse через крупные файлы и незавершённые jobs.

## 3. Webhook Hardening

Если webhook реально нужен, обязательны:

- callback signature verification
- retry policy
- idempotency
- allowlist or validation strategy
- delivery logs

Webhook нельзя считать базовой функцией, пока polling не закрыт хорошо.

## 4. Abuse Controls

Для внешнего сервиса нужны:

- tenant quotas
- per-token or per-tenant rate limits
- hard size caps
- mime validation
- request budget controls
- anti-abuse monitoring

## 5. Error Taxonomy

Нужно разделять:

- upload errors
- provider transport errors
- provider rate-limit errors
- malformed media
- timeout errors
- internal processing errors

Без этого support и observability быстро разваливаются.

## 6. Reliability Practices

Полезный минимум:

- health checks
- structured logs
- provider-specific dashboards
- latency SLOs
- alerting on failure spikes

## 7. Cost Controls

После появления реального трафика нужны:

- estimated cost tracking by provider
- blended cost tracking with fallback
- budget alerts
- provider routing rules only after usage evidence

## 8. Security Controls

Нужны:

- auth separation for internal vs external clients
- secret rotation process
- callback verification
- auditability of jobs and failures
- least-privilege storage access

## 9. Rule Of Use

Не переносите этот слой в MVP.

Production hardening нужен не раньше, чем появляются:

- реальные пользователи;
- реальные расходы;
- реальные инциденты;
- реальные требования к SLA.
