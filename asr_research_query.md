# Open Source ASR Research Query

Saved on: 2026-03-14 UTC
Purpose: reusable master prompt for continuing and expanding the ASR market/technology study.

## Master Prompt

Conduct a thorough market and engineering review of currently available open source speech-to-text (ASR/STT) solutions and answer the product decision question: should we build our own speech recognition system, fine-tune an existing model, or adopt an existing open source stack as-is.

The output must be practical, current, and decision-oriented rather than academic. Prioritize official project documentation, official GitHub repositories, model cards, and primary-source benchmark or usage documentation. Avoid low-signal blogs unless needed for context.

### Goals

1. Identify the strongest open source ASR options available now.
2. Separate inference-ready engines from research/training toolkits.
3. Compare them on production concerns, not only benchmark claims.
4. Determine when building a custom ASR stack is justified.
5. Produce a shortlist by scenario:
   - batch transcription
   - low-latency streaming
   - on-device or edge
   - multilingual
   - Russian-first
   - self-hosted/on-prem
   - mobile/browser

### Mandatory Comparison Dimensions

- Recognition quality in realistic production use
- Streaming support vs only offline/file transcription
- Latency and throughput characteristics
- CPU viability
- GPU acceleration options
- On-device/mobile/browser support
- Language coverage
- Russian-language strength
- Long-audio handling
- Timestamps / word timestamps
- VAD support
- Punctuation / ITN / diarization availability
- Fine-tuning / domain adaptation path
- Deployment complexity
- License
- Project activity and maintenance health
- Ecosystem maturity and community adoption

### Projects To Check First

- Whisper
- faster-whisper
- whisper.cpp
- Vosk
- sherpa-onnx
- FunASR
- WeNet
- GigaAM
- NVIDIA NeMo
- SpeechBrain
- Hugging Face Transformers ASR stack
- ESPnet
- PaddleSpeech
- Kaldi
- Coqui STT

### Research Method

1. Use official repositories and docs first.
2. Record concrete evidence with dates, release tags, or exact README/doc claims.
3. Distinguish stated facts from inference.
4. Track project freshness:
   - latest repo push date
   - latest release date
   - stars/forks/issues as rough maturity signals
5. Group solutions into categories:
   - ready-to-integrate inference engines
   - runtime/deployment stacks
   - training/fine-tuning frameworks
   - legacy but still relevant systems
6. Note where a project is strong but operationally expensive.

### Required Deliverables

- Executive recommendation
- Long-form comparison table
- Detailed notes per project
- Build-vs-buy decision framework
- Recommended evaluation bake-off plan
- Source appendix with links

### Important Decision Rules

- Default recommendation should not be "build from scratch" unless there is strong evidence.
- If a project is best only for a narrow case, say so explicitly.
- If a system is stale or risky, mark it as such even if historically important.
- If "best" depends on context, provide scenario-based picks instead of one universal winner.

## Initial Query Set Used In This Round

These searches were used to build the first pass of the report and should be preserved for future expansion:

- open source speech recognition GitHub Whisper repository
- NVIDIA NeMo ASR Parakeet official documentation
- Vosk speech recognition official website GitHub
- FunASR official GitHub speech recognition
- WeNet official GitHub speech recognition
- faster-whisper GitHub official
- sherpa-onnx official GitHub speech recognition
- SpeechBrain ASR official documentation
- Hugging Face transformers automatic speech recognition wav2vec2 docs
- Coqui STT GitHub official
- GigaAM open source speech recognition official GitHub
- Sber GigaAM speech recognition official
- Russian open source speech recognition model official GitHub
- site:github.com/wenet-e2e/wenet WeNet GitHub official
- site:github.com/kaldi-asr/kaldi kaldi github official
- site:huggingface.co/docs/transformers automatic speech recognition task docs
- site:docs.nvidia.com NeMo checkpoints ASR benchmark Parakeet
- Whisper paper 680000 hours arxiv
- NeMo fine-tune your own ASR dataset official
- Vosk language model adaptation official

## Expansion Backlog

For the next iteration, extend the research with:

1. Measured WER/CER comparisons on our own eval set.
2. Real-time factor on our target hardware:
   - CPU-only server
   - single NVIDIA GPU
   - Apple Silicon
3. Long-form transcript stability:
   - speaker overlap
   - punctuation drift
   - proper nouns
   - timestamps
4. Russian domain adaptation options:
   - hotwords
   - biasing
   - shallow fusion / LM integration
   - fine-tuning workflows
5. Deployment architecture templates:
   - batch worker
   - streaming websocket service
   - browser or edge local inference
6. Licensing review for commercial use.

## Output Format For Future Runs

When re-running this research, structure the answer as:

1. Current recommendation
2. What changed since previous run
3. Search findings
4. Detailed comparison
5. Scenario-based picks
6. Build vs buy decision
7. Next experiments
8. Sources
