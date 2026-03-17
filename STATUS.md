# STATUS.md

Last updated: 2026-03-16 UTC
Scope: operational status of the ASR research folder, what is complete, what is stable, what may age quickly, and what should be revisited next.

## 1. Overall Status

Project state: active research base, structurally organized, ready for extension.

The folder already contains:

- high-level landscape mapping;
- open source ASR survey;
- managed STT/API shortlist;
- backend architecture blueprint;
- layered backend architecture docs for MVP, advanced patterns, and production hardening;
- personal dictation tooling review;
- Wispr Flow comparison;
- navigation and agent handoff docs.

The current state is strong enough for:

- product direction decisions;
- shortlist discussions;
- backend architecture planning;
- follow-up benchmark work.

The current state is not yet complete for:

- hard quantitative bake-off on real audio;
- latency benchmarking on target devices;
- measured WER/CER comparisons on an owned eval set;
- implementation-specific rollout estimates.

The folder also now contains parked project ideas that are intentionally not active.

## 2. Completed Work

### Core navigation and context

- [README.md](/home/skris/research/asr/README.md)
- [AGENT_CONTEXT.md](/home/skris/research/asr/AGENT_CONTEXT.md)
- [STATUS.md](/home/skris/research/asr/STATUS.md)

### High-level decision framing

- [asr_landscape_map.md](/home/skris/research/asr/asr_landscape_map.md)

### Open source / self-host research

- [asr_open_source_survey_2026-03-14.md](/home/skris/research/asr/asr_open_source_survey_2026-03-14.md)
- [asr_research_query.md](/home/skris/research/asr/asr_research_query.md)

### Managed STT/API research

- [asr_managed_services_shortlist_2026-03-16.md](/home/skris/research/asr/asr_managed_services_shortlist_2026-03-16.md)
- [asr_provider_decision_matrix.md](/home/skris/research/asr/asr_provider_decision_matrix.md)
- [asr_backend_blueprint.md](/home/skris/research/asr/asr_backend_blueprint.md)

### Personal dictation research

- [personal_dictation_stack.md](/home/skris/research/asr/personal_dictation_stack.md)
- [wisprflow_vs_asr_options.md](/home/skris/research/asr/wisprflow_vs_asr_options.md)

## 3. Established Baselines

These are the currently established default recommendations inside this folder.

### Open source / self-host

- default batch/server baseline: `faster-whisper`
- default local/edge baseline: `whisper.cpp`
- default streaming/mobile runtime candidate: `sherpa-onnx`
- mandatory Russian benchmark candidate: `GigaAM`

### Managed/API

- cheapest managed batch baseline found: `Cloudflare Workers AI Whisper`
- default thin-wrapper backend upstream: `Groq STT`
- strongest free-start/pilot option: `AssemblyAI`
- strongest voice/realtime-oriented API option in current shortlist: `Deepgram`
- MVP backend should stay minimal; advanced backend ideas are now split into separate later-stage docs

### Personal dictation

- default polished cloud dictation recommendation: `Wispr Flow`
- default private/local-first dictation recommendation: `Superwhisper`
- default dictation + file transcription hybrid recommendation: `MacWhisper`

## 4. What Is Stable vs What Ages Quickly

### Relatively stable

These usually do not need constant revalidation:

- category taxonomy:
  - personal dictation
  - managed STT API
  - open source/self-host
- major architectural conclusions
- distinction between consumer subscriptions and backend APIs
- core open source positioning:
  - `faster-whisper` for practical batch
  - `whisper.cpp` for local/edge
  - `sherpa-onnx` for runtime-heavy streaming cases

### Ages quickly

These should be treated as time-sensitive:

- pricing
- free-tier limits
- plan entitlements
- "public API available or not"
- platform support
- release freshness
- latest benchmark claims
- availability of team/business features

## 5. Time-Sensitive Claims That Should Be Rechecked

Before using this folder for a new decision, recheck these classes of claims:

### Managed providers

- `Cloudflare` pricing and neuron math
- `Groq` STT pricing and file-size limits
- `AssemblyAI` free-tier size
- `Deepgram` pricing and add-on prices
- `OpenAI` transcription pricing

### Personal dictation tools

- `Wispr Flow` pricing and platform support
- `Superwhisper` pricing and local/cloud tier details
- `Aqua Voice` pricing and platform support
- `EdgeWhisper` pricing and language coverage
- `MacWhisper` one-time license pricing and dictation feature set

### Open source projects

- repo activity and last release dates
- major runtime feature additions
- licensing changes
- new model families or deprecations

## 6. Current Gaps

The research is still missing several high-value pieces:

### Gap A. Real evaluation data

Missing:

- own eval set
- WER/CER comparisons
- transcript quality judgments on real samples
- noisy/telephony/domain-specific checks

### Gap B. Hardware and latency measurements

Missing:

- CPU-only benchmark on target hardware
- Apple Silicon benchmark
- serverless/GPU benchmark under realistic batching
- real-time factor on small and large audio

### Gap C. Implementation details

Missing:

- code skeleton for backend service
- database schema migrations
- queue worker prototype
- provider adapter contract in code

Note:

- backend document layering is now improved, but implementation artifacts are still missing.

### Gap D. Russian-specific specialization

Missing:

- dedicated Russian dictation tool comparison
- Russian backend/API quality comparison
- Russian domain-term handling and proper nouns study

## 7. Recommended Next Actions

### Highest-value next action

Create a measurable evaluation plan and dataset:

- collect representative audio;
- create manual transcripts;
- define pass/fail quality thresholds;
- run `Groq`, `Cloudflare`, `faster-whisper`, and one Russian-focused candidate if needed.

### Second highest-value action

Build a cost calculator:

- monthly audio hours;
- provider routing assumptions;
- fallback percentages;
- estimated blended cost.

### Third highest-value action

Create implementation-ready backend materials:

- API contract;
- data model;
- worker design;
- deployment checklists.

## 8. Parked Projects

These are intentionally not active:

- generic voice-input / dictation product

See:

- [FUTURE_PROJECTS.md](/home/skris/research/asr/FUTURE_PROJECTS.md)

Reason:

- interesting category, but too crowded right now for it to be a default build priority.

## 9. Suggested Maintenance Cadence

### Recheck monthly

- managed pricing
- free tiers
- consumer dictation pricing
- major provider/API availability changes

### Recheck quarterly

- open source repo freshness
- new model releases
- strong new entrants in dictation tools
- major licensing or deployment changes

### Recheck before any real purchase or implementation

- all pricing
- all limits
- platform support
- business/team plan details
- API availability

## 10. Confidence Notes

High confidence:

- taxonomy and separation of categories
- broad recommendation against scratch-built ASR
- role of `faster-whisper`, `whisper.cpp`, `sherpa-onnx`
- role of `Groq` and `Cloudflare` in the current managed shortlist
- role of `Wispr Flow` as a dictation product rather than an API

Medium confidence:

- exact ranking among personal dictation tools
- practical superiority of one dictation app over another without direct hands-on testing
- comparative quality of some API providers without a local bake-off

Lower confidence until measured locally:

- exact quality ranking on Russian-heavy data
- exact blended economics under real fallback routing
- exact throughput crossover where self-host beats managed APIs

## 11. If Another Agent Continues This Work

Recommended startup sequence:

1. Read [README.md](/home/skris/research/asr/README.md)
2. Read [AGENT_CONTEXT.md](/home/skris/research/asr/AGENT_CONTEXT.md)
3. Read [asr_landscape_map.md](/home/skris/research/asr/asr_landscape_map.md)
4. Jump to the scenario-specific source doc
5. Revalidate only the time-sensitive claims relevant to the new task

## 12. Folder Health

Current folder health: good

Why:

- documents are structured by scenario;
- links between documents exist;
- handoff context exists;
- ownership has been normalized to `skris`;
- the folder is usable as both a research notebook and an agent handoff package.
