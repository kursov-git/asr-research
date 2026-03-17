# TASKS.md

Last updated: 2026-03-16 UTC
Purpose: prioritized execution backlog for continuing the ASR research and turning it into implementation-ready work.

## How To Use This File

This is not a generic idea list.

Each task below is intended to be:

- actionable;
- scoped;
- ordered by priority;
- usable by either a human or another LLM agent.

Priority levels:

- `P0`: highest-value next actions
- `P1`: important follow-up work
- `P2`: useful but not urgent

Status values:

- `todo`
- `in_progress`
- `blocked`
- `done`

## Current Focus

Recommended immediate focus:

1. define a measurable evaluation plan
2. create a cost model by monthly volume
3. turn the MVP backend blueprint into implementation-ready artifacts

## P0 Tasks

### P0-01: Create ASR Evaluation Plan

- Status: `todo`
- Goal: define how providers and self-host options will be evaluated on real audio
- Why it matters: current conclusions are structurally strong but not yet validated on owned data

Deliverables:

- `evaluation_plan.md`
- evaluation criteria
- pass/fail thresholds
- provider list for bake-off

Minimum scope:

- batch audio
- noisy audio
- long-form audio
- Russian-sensitive samples if relevant
- timestamps quality criteria

Definition of done:

- clear scoring dimensions exist
- test scenarios are explicit
- primary candidates are named
- decision criteria are measurable

### P0-02: Create Eval Dataset Spec

- Status: `todo`
- Goal: specify the dataset needed for a meaningful bake-off
- Why it matters: without representative audio, provider comparison stays anecdotal

Deliverables:

- `eval_dataset_spec.md`

Minimum scope:

- sample categories
- target duration per category
- transcript quality rules
- metadata schema

Recommended categories:

- clean speech
- noisy speech
- meetings
- phone-quality audio
- domain terminology
- Russian or multilingual samples if applicable

Definition of done:

- dataset composition is explicit
- collection rules are explicit
- labeling expectations are explicit

### P0-03: Create Managed Cost Calculator Spec

- Status: `todo`
- Goal: define a reusable cost model for managed STT providers
- Why it matters: pricing pages exist, but blended monthly economics are not yet modeled

Deliverables:

- `cost_calculator_spec.md`

Minimum scope:

- providers included
- cost inputs
- fallback-rate inputs
- output tables

Required scenarios:

- 100 hours/month
- 1,000 hours/month
- 10,000 hours/month

Definition of done:

- formulas are explicit
- assumptions are explicit
- provider routing can be modeled

### P0-04: Define Vendor-Agnostic API Contract

- Status: `todo`
- Goal: turn the backend blueprint into a provider-neutral API contract
- Goal: turn the backend docs into a provider-neutral API contract without polluting the MVP layer
- Why it matters: implementation should not hard-bind business logic to one provider

Deliverables:

- `api_contract.md`
- request/response schemas
- normalized segment and word structures
- error contract

Definition of done:

- job creation contract is defined
- status contract is defined
- result contract is defined
- webhook contract is defined
- provider-neutral output shape exists

## P1 Tasks

### P1-01: FastAPI Implementation Checklist

- Status: `todo`
- Goal: convert the backend blueprint into a Python implementation checklist

Deliverables:

- `fastapi_implementation_checklist.md`

Definition of done:

- components are mapped to modules
- infra dependencies are listed
- MVP milestones are explicit

### P1-02: NestJS Implementation Checklist

- Status: `todo`
- Goal: provide an equivalent implementation checklist for TypeScript teams

Deliverables:

- `nestjs_implementation_checklist.md`

Definition of done:

- components are mapped to modules
- queue/storage/auth choices are explicit

### P1-03: Russian Dictation Review

- Status: `todo`
- Goal: produce a focused review of dictation tools for Russian-heavy usage
- Why it matters: current dictation review is broad, but not yet RU-specific

Deliverables:

- `russian_dictation_tools.md`

Definition of done:

- shortlist is narrowed to Russian-relevant tools
- risks and unknowns are documented

### P1-04: Russian Backend/API Review

- Status: `todo`
- Goal: compare managed and self-host options specifically for Russian transcription quality

Deliverables:

- `russian_asr_backend_shortlist.md`

Definition of done:

- RU-relevant providers and open source candidates are compared
- GigaAM position is clarified

### P1-05: Benchmark Plan For Mac Dictation UX

- Status: `todo`
- Goal: define a practical comparison plan for Wispr Flow, Superwhisper, Aqua Voice, EdgeWhisper, and MacWhisper

Deliverables:

- `mac_dictation_benchmark_plan.md`

Definition of done:

- latency criteria exist
- correction-rate criteria exist
- workflow scenarios exist

## P2 Tasks

### P2-01: Folder Glossary

- Status: `todo`
- Goal: create a glossary of recurring ASR terms for future agents and humans

Deliverables:

- `glossary.md`

### P2-02: Research Update Template

- Status: `todo`
- Goal: create a template for future dated updates

Deliverables:

- `update_template.md`

### P2-03: Purchase Decision Checklist

- Status: `todo`
- Goal: create a final pre-buy checklist for subscriptions, APIs, or self-host

Deliverables:

- `purchase_checklist.md`

## Dependencies

Recommended ordering:

1. `P0-01` evaluation plan
2. `P0-02` eval dataset spec
3. `P0-03` cost calculator spec
4. `P0-04` vendor-agnostic API contract
5. `P1-01` or `P1-02` implementation checklist

Why:

- evaluation comes before strong claims;
- cost modeling comes before provider commitment;
- API contract comes before implementation.

## Suggested Owners

If another agent picks up work:

- research-heavy agent:
  - `P0-01`
  - `P0-02`
  - `P1-03`
  - `P1-04`
  - `P1-05`
- implementation-heavy agent:
  - `P0-04`
  - `P1-01`
  - `P1-02`
- product/ops-heavy agent:
  - `P0-03`
  - `P2-03`

## Notes

- Do not reopen broad category questions unless new evidence appears.
- Prefer incremental extension of the existing folder structure.
- Revalidate time-sensitive claims before any pricing or purchase recommendation.
