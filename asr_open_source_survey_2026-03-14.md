# Open Source ASR Survey

Date: 2026-03-14 UTC
Scope: current open source speech-to-text options for self-hosted or embedded use, with emphasis on practical adoption rather than academic novelty.

## Executive Summary

The default recommendation is:

- do not build ASR from scratch;
- start from an existing open source engine;
- add your own surrounding pipeline;
- fine-tune only if your domain data proves the baseline is insufficient.

For most teams, the right first step is one of these:

- `faster-whisper` for general batch/server transcription;
- `whisper.cpp` for local, CPU, Apple Silicon, and edge/browser scenarios;
- `sherpa-onnx` when low-latency streaming and multi-platform runtime matter most;
- `GigaAM` as a mandatory comparison point if Russian is a primary language.

My conclusion is that "build our own" is rarely justified. A more realistic and economically sound path is "assemble our own ASR product stack around an existing model/runtime".

## Methodology

This review used primary sources only:

- official GitHub repositories;
- official documentation sites;
- model/toolkit documentation;
- GitHub API metadata for repo activity and latest releases.

The comparison was made across these dimensions:

- production readiness;
- recognition quality claims and scope;
- streaming support;
- offline/on-device support;
- deployment targets;
- fine-tuning path;
- language coverage;
- Russian-language relevance;
- maintenance activity and release freshness;
- licensing.

Where I make a judgment like "likely weaker quality" or "higher operational complexity", that is an inference based on the architecture, ecosystem maturity, and project positioning, not a direct quote from maintainers.

## Search Findings

### 1. The market has split into three clear categories

1. Inference-first runtimes:
   `faster-whisper`, `whisper.cpp`, `Vosk`, `sherpa-onnx`
2. Runtime plus broader speech stack:
   `FunASR`, `WeNet`, partly `GigaAM`
3. Training/fine-tuning frameworks:
   `NVIDIA NeMo`, `SpeechBrain`, `ESPnet`, `Transformers`, `PaddleSpeech`, `Kaldi`

This matters because many teams mistakenly compare a deployable runtime to a research framework. They solve different problems.

### 2. Whisper is still the reference point, but no longer the only practical choice

`Whisper` remains the baseline people compare against because it is multilingual, robust, and widely integrated. The official README states that it is trained on a "large dataset of diverse audio", supports multilingual ASR, speech translation, and language identification, and processes audio with a sliding 30-second window.

However, deployment choices have diversified:

- `faster-whisper` focuses on efficient server inference;
- `whisper.cpp` focuses on portable local inference;
- `sherpa-onnx` focuses on streaming and cross-platform runtimes;
- `GigaAM` is a serious Russian-focused alternative.

### 3. Streaming is a separate product requirement, not a box-check feature

If true low-latency streaming is required, inference wrappers around offline models are often not enough. The strongest evidence-backed options here are:

- `sherpa-onnx`: explicit support for both streaming and non-streaming ASR plus mobile/WASM targets;
- `FunASR`: real-time service, streaming models, VAD integration, chunk-based latency controls;
- `WeNet`: production-oriented toolkit explicitly built for streaming and non-streaming speech recognition.

### 4. Russian support changes the shortlist

If Russian is not central, Whisper-family tools are usually enough for the first evaluation round.

If Russian is central, `GigaAM` should be included immediately. Its README positions it as a Russian foundational model, reports v2/v3 improvements, and gives concrete training-hour figures:

- v2: 50,000 pretrain hours and 2,000 ASR hours
- v3: 700,000 pretrain hours and 4,000 ASR hours

That makes it too relevant to ignore in a Russian-first stack.

### 5. Some historically important projects are no longer the best greenfield choice

- `Kaldi` is historically foundational and still relevant as a legacy ecosystem, but it is not the easiest modern starting point.
- `Coqui STT` appears materially less fresh than current leaders, with its latest GitHub release dated 2022-09-03 and repo push activity from 2024-03-11.

## Project Activity Snapshot

Signals below were checked on 2026-03-14 UTC via GitHub API. These are rough maintenance indicators, not quality guarantees.

| Project | Stars | Open Issues | Last Push | Latest Release | License | Comment |
|---|---:|---:|---|---|---|---|
| Whisper | 95,920 | 115 | 2025-12-15 | 2025-06-26 | MIT | Widely adopted reference model |
| faster-whisper | 21,457 | 308 | 2025-11-19 | 2025-10-31 | MIT | Active inference-focused implementation |
| whisper.cpp | 47,518 | 1,156 | 2026-03-05 | 2026-01-15 | MIT | Very active deployment/runtime ecosystem |
| Vosk | 14,377 | 589 | 2026-02-22 | 2024-04-22 | Apache-2.0 | Mature, practical offline toolkit |
| sherpa-onnx | 10,788 | 574 | 2026-03-13 | 2026-03-12 | Apache-2.0 | Very active streaming/runtime stack |
| FunASR | 15,204 | 538 | 2026-03-12 | 2023-03-16 | MIT | Active repo, release cadence less clear |
| WeNet | 5,053 | 20 | 2025-12-19 | 2024-05-23 | Apache-2.0 | Focused, production-oriented toolkit |
| GigaAM | 499 | 12 | 2026-02-12 | no GitHub release found | MIT | Small repo, important RU specialization |
| NeMo | 16,901 | 183 | 2026-03-14 | n/a in this pass | Apache-2.0 | Strong training and enterprise deployment story |
| SpeechBrain | 11,314 | 174 | 2026-03-01 | 2025-04-07 | Apache-2.0 | Mature research and fine-tuning framework |
| ESPnet | 9,769 | 51 | 2026-03-14 | n/a in this pass | Apache-2.0 | Strong academic/engineering toolkit |
| PaddleSpeech | 12,553 | 264 | 2026-03-02 | n/a in this pass | Apache-2.0 | Broad speech stack, strong CN ecosystem |
| Coqui STT | 2,572 | 107 | 2024-03-11 | 2022-09-03 | MPL-2.0 | Freshness concern for new adoption |
| Kaldi | 15,341 | 254 | 2025-09-22 | n/a in this pass | License metadata unclear in API | Legacy heavyweight |

## Shortlist Comparison

### Inference-Ready Options

| Solution | Best Fit | Strengths | Weaknesses | Streaming | On-device | Fine-tune Path |
|---|---|---|---|---|---|---|
| faster-whisper | server-side batch transcription | fast, memory-efficient, simple Python integration, Whisper-quality compatibility | not a full end-to-end streaming platform by itself | partial, typically wrapped via chunking/VAD | limited vs C++ runtimes | via Whisper ecosystem, but not its primary role |
| whisper.cpp | CPU/edge/local/browser | C/C++, quantization, Apple Silicon, Core ML, WebAssembly, built-in examples for stream/server/VAD | engineering around it is still your job | yes, example-based | excellent | indirect, usually train elsewhere and deploy here |
| Vosk | lightweight offline integration | small models, streaming API, vocabulary adaptation, broad language/platform support | likely lower ceiling than newer large-model stacks | yes | good | adaptation exists, but less modern than newer toolkits |
| sherpa-onnx | low-latency cross-platform runtime | streaming + non-streaming, Android/iOS/WASM, VAD, broad runtime coverage | more systems-oriented, somewhat higher integration complexity | strong | excellent | model ecosystem is broader than one architecture, training often external |
| FunASR | richer speech stack with realtime support | ASR, VAD, punctuation, diarization, ONNX, streaming docs, fine-tuning scripts | broader stack means higher complexity; ecosystem less universal in Western stacks | strong | decent | yes |
| WeNet | production speech stack for streaming | production-first positioning, runtime, training + deployment path | less plug-and-play for a quick server baseline | strong | moderate | yes |
| GigaAM | Russian-first deployments | Russian specialization, ONNX export, CTC/RNNT variants, current development | smaller ecosystem, must validate tooling fit | limited evidence in this pass for streaming maturity | potentially via ONNX | yes |

### Training / Fine-Tuning Toolkits

| Solution | Role | What It Is Good For | Tradeoff |
|---|---|---|---|
| NVIDIA NeMo | enterprise-grade training and deployment framework | strong pretrained ASR models, timestamps, real-time tutorials, train-your-own path | heavier stack, more MLOps burden |
| SpeechBrain | accessible research/training toolkit | recipes, easy fine-tuning, many pretrained assets | less opinionated as a production runtime |
| Transformers | model access and quick fine-tuning | standard HF workflow, broad ecosystem, easy experimentation | production runtime and speech pipeline still need assembly |
| ESPnet | comprehensive speech research toolkit | many recipes, offline/streaming interface, reproducible experiments | steeper learning curve |
| PaddleSpeech | broad speech platform | production-ready streaming claims, broad demos | ecosystem fit depends on team stack and language needs |
| Kaldi | legacy industrial toolkit | classical ASR stack, still useful for some specialized workflows | complexity and age make it a weak default for greenfield |

## Detailed Notes By Project

### Whisper

Evidence from the official README:

- multilingual ASR, speech translation, and language identification;
- trained on a large and diverse dataset;
- `transcribe()` processes audio with a sliding 30-second window;
- `turbo` is optimized for English transcription but not translation.

Assessment:

- still the conceptual baseline;
- excellent for offline file transcription and general robustness;
- not the cleanest foundation if low-latency streaming is the top requirement.

### faster-whisper

Evidence from the official README:

- "up to 4 times faster" than `openai/whisper` for the same accuracy;
- lower memory usage;
- supports efficient batched transcription;
- supports quantization on CPU and GPU.

Assessment:

- best default server-side starting point if you want Whisper-class results without paying the full original inference cost;
- usually the first thing to benchmark unless your target is edge/mobile.

### whisper.cpp

Evidence from the official README:

- first-class Apple Silicon optimizations;
- `Core ML` support for encoder inference on ANE;
- `WebAssembly` support;
- `whisper-server` example;
- `stream` example for continuous transcription;
- optional `VAD` support.

Assessment:

- strongest local-runtime choice if you need deploy-anywhere behavior;
- especially attractive for Apple Silicon, desktop apps, embedded tools, and browser-adjacent workflows;
- less opinionated than a full speech service, so you still design the surrounding product logic.

### Vosk

Evidence from the official site/README:

- offline toolkit;
- 20+ languages and dialects;
- small models around 50 MB;
- streaming API with zero-latency response wording in README;
- reconfigurable vocabulary and speaker identification;
- runs from Raspberry Pi and Android up to servers.

Assessment:

- still very practical if simplicity, small footprint, and offline support matter more than peak accuracy;
- likely not the best quality ceiling for a modern greenfield transcription product.

### sherpa-onnx

Evidence from the official README:

- speech-to-text with both streaming and non-streaming support;
- VAD support;
- Android, iOS, Windows, macOS, Linux, HarmonyOS targets;
- WebAssembly support;
- extensive pretrained model/runtime examples.

Assessment:

- one of the strongest choices when the system requirement is "real streaming across many runtime targets";
- stronger engineering fit than Whisper-family wrappers for mobile and embedded runtime distribution.

### FunASR

Evidence from the official README:

- offers ASR, VAD, punctuation restoration, language models, speaker verification, speaker diarization, multi-talker ASR;
- supports inference and fine-tuning;
- supports ONNX export;
- documents real-time transcription service and chunk-based streaming latency;
- recent notes mention low-latency real-time transcription and 31-language coverage for newer lines.

Assessment:

- compelling if you want more than plain ASR;
- a good candidate for richer speech products with a self-hosted stack;
- more ambitious and therefore operationally heavier than a narrow inference wrapper.

### WeNet

Evidence from the official README:

- describes itself as "production first and production ready";
- explicitly targets streaming and non-streaming end-to-end speech recognition;
- includes runtime and deployment build paths.

Assessment:

- strong candidate for teams ready to adopt a more end-to-end speech platform;
- less ideal for "we need a baseline this week" than `faster-whisper`.

### GigaAM

Evidence from the official README:

- Russian foundational Conformer-based model;
- v2 and v3 generations with WER reductions called out;
- CTC and RNN-T model lines;
- ONNX export and inference documented;
- training scale data reported for v2 and v3.

Assessment:

- mandatory benchmark candidate if Russian is primary;
- not enough evidence in this pass to call it the universal default outside Russian-heavy use cases.

### NVIDIA NeMo

Evidence from official docs:

- supports open-sourced pretrained models in 14+ languages;
- "transcribe speech with 3 lines of code";
- timestamps available for Parakeet models;
- provides train-your-own guidance;
- provides real-time ASR tutorial notebooks.

Assessment:

- best viewed as the serious upgrade path when off-the-shelf inference is not enough;
- suitable if you expect to own training, evaluation, and deployment over time.

### SpeechBrain

Evidence from the official README:

- supports training from scratch and fine-tuning pretrained models including Whisper, Wav2Vec2, WavLM, and HuBERT;
- offers many recipes and pretrained models;
- inference can be done in a few lines of code.

Assessment:

- strong fine-tuning/research choice;
- less compelling as the single runtime answer for production deployment.

### Transformers ASR Stack

Evidence from official docs:

- standard tutorial path for fine-tuning Wav2Vec2 on MInDS-14;
- inference through the ASR pipeline;
- broad access to open ASR models.

Assessment:

- ideal for experimentation and model-level iteration;
- not by itself a full speech product stack.

### ESPnet

Evidence from the official README:

- end-to-end speech processing toolkit;
- many ASR recipes;
- unified interface for offline and streaming speech recognition.

Assessment:

- serious toolkit for teams with research depth;
- rarely the shortest path to a production MVP.

### PaddleSpeech

Evidence from the official README:

- "production ready streaming asr and streaming tts system";
- CLI, server, and streaming server quick-start paths;
- broad demos and examples for ASR workloads.

Assessment:

- worth evaluating if your team already works well in that ecosystem or has strong Chinese-language requirements;
- otherwise not the clearest default over more globally common stacks.

### Kaldi

Assessment:

- historically one of the most important ASR toolkits;
- still relevant for some classical pipelines and older production systems;
- not recommended as the default choice for a new project unless there is a specific legacy or decoder reason.

### Coqui STT

Assessment:

- historically notable due to the DeepSpeech lineage;
- in this 2026 review it does not look like the strongest greenfield choice due to weaker freshness signals compared with current alternatives.

## Scenario-Based Recommendations

### If we need a general self-hosted transcription service

Start with:

1. `faster-whisper`
2. `GigaAM` if Russian is important
3. `whisper.cpp` only if CPU/local deployment dominates

Why:

- fastest path to a credible baseline;
- easy to test on real audio quickly;
- lowest decision regret.

### If we need true low-latency streaming

Start with:

1. `sherpa-onnx`
2. `FunASR`
3. `WeNet`

Why:

- these projects treat streaming as a first-class system design concern rather than an afterthought.

### If we need on-device, desktop, mobile, or browser-friendly deployment

Start with:

1. `whisper.cpp`
2. `sherpa-onnx`
3. `Vosk` for lightweight legacy-style offline needs

### If Russian is core to the product

Mandatory bake-off:

1. `GigaAM`
2. `faster-whisper`
3. `whisper.cpp` or `sherpa-onnx` depending on runtime target

Why:

- Russian specialization changes the ranking enough that a generic global default is not sufficient.

## Build vs Buy Decision

### Do not build from scratch if:

- the task is general transcription;
- the main requirement is self-hosting/privacy;
- you do not have a very large labeled in-domain speech corpus;
- you do not already operate ML training infrastructure for speech;
- latency/accuracy tradeoffs can be solved by choosing the right existing runtime.

### Build your own stack around an existing model if:

- you need custom chunking, retries, long-audio handling, post-processing, confidence logic, hotwording, analytics, and product-specific formatting;
- you need to integrate diarization, punctuation, and domain normalization;
- you need self-hosted APIs, queues, storage, and observability.

This is the most realistic meaning of "our own ASR system" for most teams.

### Fine-tune an existing model if:

- your audio domain is specialized;
- names, jargon, or accents consistently fail on the best baseline;
- there is a measurable gap on your own eval set;
- you can afford ongoing data curation and retraining.

### Consider true from-scratch training only if all of the following are true:

1. you have large proprietary speech corpora and labels;
2. off-the-shelf models and fine-tuning both fail materially;
3. the business value of the gap is high enough to justify a continuing speech-ML program.

## Recommended Evaluation Plan

Before any architectural commitment, run a bake-off on your own data.

### Minimum dataset

- 2 to 10 hours of representative audio
- manually verified transcripts
- cover noise, long-form speech, accents, domain terms, and silence/music boundaries

### Metrics

- WER and CER
- real-time factor
- end-to-end latency
- timestamp quality
- punctuation quality
- proper-noun accuracy
- stability on long-form audio
- operational complexity

### First bake-off set

- `faster-whisper`
- `GigaAM` if Russian matters
- `sherpa-onnx` if streaming matters
- `whisper.cpp` if local/offline runtime matters

### Decision rule

Adopt the simplest stack that passes:

- target quality;
- target latency;
- target deployment constraints;
- target licensing/commercial constraints.

Only move to training/fine-tuning after this first pass.

## Bottom-Line Recommendation

If no additional constraints are given, the practical recommendation is:

1. Use `faster-whisper` as the default baseline.
2. Add `GigaAM` to the benchmark if Russian is important.
3. Use `sherpa-onnx` instead if true streaming or embedded/mobile runtime is the main requirement.
4. Use `whisper.cpp` if offline local deployment is central.
5. Do not start a scratch-built ASR effort.

## Sources

Primary sources used in this review:

- Whisper: https://github.com/openai/whisper
- Whisper research page: https://openai.com/research/whisper
- faster-whisper: https://github.com/SYSTRAN/faster-whisper
- whisper.cpp: https://github.com/ggml-org/whisper.cpp
- Vosk website: https://alphacephei.com/vosk/
- Vosk repo: https://github.com/alphacep/vosk-api
- sherpa-onnx: https://github.com/k2-fsa/sherpa-onnx
- FunASR: https://github.com/modelscope/FunASR
- WeNet: https://github.com/wenet-e2e/wenet
- GigaAM: https://github.com/salute-developers/GigaAM
- NVIDIA NeMo ASR docs: https://docs.nvidia.com/nemo-framework/user-guide/latest/nemotoolkit/asr/intro.html
- SpeechBrain: https://github.com/speechbrain/speechbrain
- Hugging Face ASR docs: https://huggingface.co/docs/transformers/tasks/asr
- ESPnet: https://github.com/espnet/espnet
- PaddleSpeech: https://github.com/PaddlePaddle/PaddleSpeech
- Kaldi: https://github.com/kaldi-asr/kaldi
- Coqui STT: https://github.com/coqui-ai/STT
