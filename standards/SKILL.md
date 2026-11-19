---
name: standards
description: The mandatory standards every project repo (dataset, ai-engine, backend-server, sim-demo, live-demo, docs, harness) must follow - shared vocabulary, autonomy by risk, safety, privacy, run log, AI output and per-repo obligations. Read it before writing, reviewing or changing code, models, data or docs in any of these repos.
---

# Project standards

These rules are mandatory. They come from the proposal *NOVA: Non-connected Object Vision and Actuation for Smart Home Routines* (https://sweg9.github.io/docs/papers/proposal) and its requirements FR-01 to FR-12 (https://sweg9.github.io/docs/product/requirements). They make the seven repos work as one system. A rule has an id so that reviews, tests and the harness can cite it.

| File | Content |
|---|---|
| [common.md](common.md) | Rules C-01 to C-13 for every repo |
| [data-model.md](data-model.md) | Records, identifiers and the shared enumerations |
| [execution-log.md](execution-log.md) | The run log every run must write (FR-12) |
| [ai-output.md](ai-output.md) | Rules A-01 to A-08 for anything a model produces |
| [per-repo.md](per-repo.md) | What each repo owns and must satisfy, with known gaps |

## How to apply

1. Find the repo you are in and read its section in `per-repo.md`, then `common.md`.
2. Use the enumerations in `data-model.md` exactly as spelled. Do not add synonyms.
3. When a change breaks a rule, do not work around it. Change the standard first, in this repo, in its own commit, and say why.
4. Cite rule ids in review comments, for example "violates C-04".
5. Safety (C-04) and privacy (C-05) have no exceptions and no "later".

When the proposal changes, these standards change with it. The numbers in the proposal are starting values and live in settings files (C-12).
