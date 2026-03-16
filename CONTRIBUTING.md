# Contributing

This repository is a research and decision-support knowledge base about speech-to-text systems.

It is organized around three distinct tracks:

1. Personal dictation tools
2. Managed STT APIs
3. Open source / self-hosted ASR

The most important contribution rule is:

- do not collapse these three tracks into one comparison

## Before You Add Anything

Read these first:

1. [README.md](/home/skris/research/asr/README.md)
2. [AGENT_CONTEXT.md](/home/skris/research/asr/AGENT_CONTEXT.md)
3. [STATUS.md](/home/skris/research/asr/STATUS.md)
4. [TASKS.md](/home/skris/research/asr/TASKS.md)

Then identify which category your contribution belongs to:

- personal dictation
- managed STT API
- open source/self-host
- project meta

## Contribution Standards

Use these rules for every update:

- prefer primary sources
- include concrete dates for unstable facts
- separate facts from inference
- record pricing with units
- do not assume consumer subscriptions include API credits
- do not say "best" without attaching it to a scenario
- avoid mixing per-month subscription pricing with per-audio-hour API pricing

## File Naming

Use descriptive snake_case names.

If the document depends on current pricing, plan limits, releases, or platform support:

- include a date in the file content
- use a dated filename when the content is a time-specific snapshot

Examples:

- `asr_managed_services_shortlist_2026-03-16.md`
- `russian_asr_backend_shortlist.md`
- `evaluation_plan.md`

## Preferred Document Structure

When possible, use this shape:

1. Scope / question
2. Product class or scenario
3. Verified findings
4. Comparison table
5. Practical recommendation
6. Sources

Helpful optional sections:

- "What this is not"
- "What not to compare directly"
- "Next research steps"

## Updating Existing Documents

Prefer extending the current structure over creating overlapping files.

When updating a time-sensitive document:

- keep the date explicit
- revalidate pricing and plan limits
- update any affected recommendation sections
- update [STATUS.md](/home/skris/research/asr/STATUS.md) if the baseline changes materially

If you add a new top-level research note:

- add it to [README.md](/home/skris/research/asr/README.md) if it changes navigation
- add it to [catalog.json](/home/skris/research/asr/catalog.json)
- add it to [index.csv](/home/skris/research/asr/index.csv)
- consider whether [AGENT_CONTEXT.md](/home/skris/research/asr/AGENT_CONTEXT.md) should mention it

## Good Contribution Targets

High-value next contributions are already listed in [TASKS.md](/home/skris/research/asr/TASKS.md).

Best starting points:

- evaluation plan
- eval dataset spec
- cost calculator spec
- vendor-agnostic API contract
- Russian-specific follow-up research

## Pull Request Guidance

A good contribution should answer:

- what scenario does this help with?
- what changed?
- what sources were used?
- which claims are time-sensitive?
- does this alter any current baseline recommendation?

If the answer changes a baseline recommendation, say so explicitly.
