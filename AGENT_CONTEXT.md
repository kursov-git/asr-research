# AGENT_CONTEXT.md

Last updated: 2026-03-16 UTC
Scope: shared context for LLM agents continuing the ASR research/project in `/home/skris/research/asr`.

## 1. Mission

This folder contains a structured research project about speech-to-text options.

The project has three distinct tracks:

1. Personal dictation tools
2. Managed STT APIs for building a product without owning compute
3. Open source / self-hosted ASR stacks

The most important rule is:

- do not collapse these three tracks into one comparison

They serve different users, different architectures, and different budget models.

## 2. Current State

The research has already produced:

- a broad open source survey;
- a managed-services shortlist;
- a provider decision matrix;
- a backend architecture blueprint;
- a Wispr Flow comparison;
- a personal dictation stack review;
- a top-level landscape map.

This means a future agent should usually extend or refine the work, not restart the comparison from zero.

## 3. Source Of Truth Documents

Read these first, in order:

1. [README.md](/home/skris/research/asr/README.md)
2. [asr_landscape_map.md](/home/skris/research/asr/asr_landscape_map.md)
3. The document matching the requested scenario

Scenario-specific source documents:

- open source/self-host:
  - [asr_open_source_survey_2026-03-14.md](/home/skris/research/asr/asr_open_source_survey_2026-03-14.md)
  - [asr_research_query.md](/home/skris/research/asr/asr_research_query.md)
- managed/API:
  - [asr_managed_services_shortlist_2026-03-16.md](/home/skris/research/asr/asr_managed_services_shortlist_2026-03-16.md)
  - [asr_provider_decision_matrix.md](/home/skris/research/asr/asr_provider_decision_matrix.md)
  - [asr_backend_blueprint.md](/home/skris/research/asr/asr_backend_blueprint.md)
- personal dictation:
  - [personal_dictation_stack.md](/home/skris/research/asr/personal_dictation_stack.md)
  - [wisprflow_vs_asr_options.md](/home/skris/research/asr/wisprflow_vs_asr_options.md)

## 4. Working Taxonomy

Always classify a user request into one of these buckets before researching:

### Bucket A. Personal Dictation

Examples:

- "I want to dictate into IDE / browser / Slack"
- "Which dictation app should I personally use?"
- "Compare Wispr Flow with Superwhisper"

Typical solutions:

- Wispr Flow
- Superwhisper
- Aqua Voice
- EdgeWhisper
- MacWhisper

### Bucket B. Managed STT API / Product Backend

Examples:

- "I need a speech-to-text backend"
- "No GPU available, what service should we use?"
- "Which API is cheapest for audio transcription?"

Typical solutions:

- Groq
- Cloudflare Workers AI
- AssemblyAI
- Deepgram
- OpenAI Transcription API

### Bucket C. Open Source / Self-Hosted

Examples:

- "Should we build our own ASR?"
- "What open source engine should we self-host?"
- "We need offline/private inference"

Typical solutions:

- faster-whisper
- whisper.cpp
- sherpa-onnx
- FunASR
- WeNet
- GigaAM

## 5. Established Conclusions

These conclusions have already been reached and should not be rediscovered from scratch unless new evidence invalidates them.

### 5.1 General conclusions

- Building ASR from scratch is usually not justified.
- A realistic "own ASR system" usually means owning the product/backend layer around an existing model or API.
- Product decisions must separate dictation tools from APIs from self-host stacks.

### 5.2 Open source conclusions

- `faster-whisper` is the default practical batch/server baseline.
- `whisper.cpp` is the default local/edge/Apple-Silicon-style baseline.
- `sherpa-onnx` is a strong streaming/mobile/runtime candidate.
- `GigaAM` must be considered if Russian is central.

### 5.3 Managed API conclusions

- `Cloudflare Workers AI Whisper` is the cheapest managed batch option found so far.
- `Groq` is the strongest thin-wrapper backend default.
- `AssemblyAI` is the strongest free-start/pilot option.
- `Deepgram` is especially relevant for voice/realtime product scenarios.

### 5.4 Personal dictation conclusions

- `Wispr Flow` is the strongest polished cloud dictation SaaS in the current research set.
- `Superwhisper` is a strong private/local-first dictation option.
- `MacWhisper` is especially strong when dictation and file transcription both matter.

## 6. Do Not Make These Mistakes

- Do not compare end-user dictation subscriptions directly against backend STT APIs without stating they are different product classes.
- Do not recommend OpenAI Plus or Google AI Pro as if they were general STT platform subscriptions for a product backend.
- Do not assume a consumer subscription includes API credits unless explicitly verified in official docs.
- Do not claim "best" without attaching it to a scenario.
- Do not rely on blogs when official docs, pricing pages, and GitHub repos exist.

## 7. Research Quality Bar

When extending this project, follow these rules:

- use primary sources first;
- include concrete dates;
- distinguish facts from inference;
- record pricing with units;
- record free-tier limits separately from list pricing;
- avoid mixing monthly subscription cost with per-audio-hour API cost;
- preserve scenario-based recommendations instead of one universal winner.

For unstable topics, always verify:

- pricing;
- plan entitlements;
- API availability;
- release freshness;
- platform support.

## 8. Preferred Output Pattern

When adding a new document, prefer this structure:

1. Scope / question
2. Product class or scenario
3. Verified findings
4. Comparison table
5. Practical recommendation
6. Sources

When helpful, add:

- "What this is not"
- "What not to compare directly"
- "Next research steps"

## 9. File Creation Rules

When adding a new research note:

- place it inside `/home/skris/research/asr`
- use a descriptive snake_case file name
- include a date in the file if the content depends on current pricing or product state
- preserve cross-links from new documents to existing source-of-truth docs
- keep the folder navigable via [README.md](/home/skris/research/asr/README.md)

After creating files:

- ensure ownership is `skris:skris`

## 10. Good Next Tasks

High-value next steps for future agents:

- create a benchmark/evaluation plan on real audio samples
- create a cost calculator by monthly audio volume
- create a Russian-language dictation-specific review
- compare latency and UX of personal dictation tools on Mac
- define a vendor-agnostic ASR API contract for implementation
- create implementation checklists for FastAPI or NestJS backend

## 11. Short Handoff Summary

If you only have one minute:

- The folder is already structured.
- Start from [README.md](/home/skris/research/asr/README.md).
- Classify the request into personal dictation, managed API, or self-host.
- Reuse existing conclusions unless the task explicitly asks to update them.
- Extend with primary-source verification and scenario-based recommendations.
