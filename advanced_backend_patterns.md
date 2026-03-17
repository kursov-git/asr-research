# Advanced Backend Patterns

Дата: 2026-03-17 UTC
Назначение: сохранить ценные backend-идеи, которые не должны сидеть в MVP, но понадобятся на следующем уровне зрелости.

## Scope

Этот документ начинается после того, как:

- один provider уже работает;
- async job flow уже стабилен;
- basic polling API уже существует.

Это слой `v0.2`.

## 1. Fallback Strategy

Fallback полезен, но только после базовой стабильности.

Рекомендуемый порядок:

1. retry на том же provider при временной ошибке
2. fallback provider при hard failure
3. premium fallback только для сложных случаев

Хорошие сигналы для эскалации:

- upstream timeout
- provider 5xx
- empty transcript
- transcript suspiciously short for audio length
- file exceeds primary provider limit

Важно:

- не включать автоматический fallback без измерений на своих данных;
- fallback легко увеличивает стоимость и делает поведение менее предсказуемым.

## 2. Provider Abstraction Layer

Делать abstraction layer стоит только тогда, когда реально появляется второй provider.

Унифицированный интерфейс:

```ts
interface AsrProvider {
  transcribe(input: TranscribeInput): Promise<TranscribeOutput>;
}
```

Нормализованный output:

- `text`
- `segments`
- `words`
- `language`
- `provider`
- `raw`

Важное предупреждение:

- abstraction layer будет leaky;
- разные провайдеры по-разному трактуют timestamps, segments и formatting;
- нельзя обещать полную взаимозаменяемость.

## 3. Chunking For Long Audio

Chunking нужен, когда:

- файлы становятся длиннее provider limits;
- появляются длинные встречи, интервью, подкасты;
- batch processing становится реальным use case.

Рекомендуемый подход:

1. если файл короткий и влезает в limit -> отправлять целиком
2. если длинный -> chunking

Стартовые параметры:

- chunk length: `5-15 min`
- overlap: `1-3 sec`
- VAD-based segmentation для noisy audio

Проблемы, которые появляются сразу:

- duplicated overlap text
- offset correction
- нестабильная пунктуация между chunk'ами

## 4. Richer Post-Processing

После MVP можно добавлять:

- punctuation normalization
- duplicate overlap cleanup
- timestamp offset correction
- language consistency checks

Не стоит рано добавлять:

- LLM semantic rewriting
- meaning-changing cleanup
- агрессивную нормализацию имён без словаря

## 5. Expanded Observability

После MVP нужны:

- upstream latency by provider
- fallback rate
- average transcript length
- estimated cost by provider
- empty transcript count
- provider-specific failure burst alerts

Цель:

- понять, где деградирует качество;
- понять, где деградирует экономика;
- видеть, когда fallback начинает флапать.

## 6. Richer API Surface

После базовой версии можно добавлять:

- webhook callbacks
- timestamps selection
- priority levels
- callback URL
- language hints

Но только после того, как basic polling path доказал стабильность.

## 7. Better Schema Additions

Позже можно добавлять:

### `transcription_jobs`

- `language_hint`
- `provider_strategy`
- `fallback_count`
- `requested_timestamps`
- `requested_diarization`

### `transcription_results`

- `segments_json`
- `words_json`
- `cost_estimate_usd`
- `processing_time_ms`

### `transcription_attempts`

- `provider`
- `attempt_number`
- `response_time_ms`
- `error_code`
- `error_message`

## Rule Of Use

Если какая-то идея отсюда не помогает вашей следующей конкретной проблеме, не добавляйте её просто потому, что она архитектурно красивая.

## Next Layer

Для следующего уровня смотрите:

- [production_hardening.md](/home/skris/research/asr/production_hardening.md)
