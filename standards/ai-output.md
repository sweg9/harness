# AI output rules

Applies to every model and check in `ai-engine` and to every caller of it. Model ids and how each one works: https://sweg9.github.io/docs/product/models (M-S1a, M-S1b, M-S2, M-S5).

All recognition runs in the Nitro Enclave on AWS (decision of 2026-11-20). The proposal sends crops to a cloud VLM; the team replaced it with its own models (decision of 2026-11-17), so the system calls no model API.

| Id | Rule |
|---|---|
| A-01 | Every model step returns a typed result that code checks. A result with a missing field or a value out of range is rejected. |
| A-02 | Every result has `confidence` (0 to 1) and `reason` (one short sentence from a fixed template, such as "brightness rose 42%"). |
| A-03 | A step answers `unsure` (a state of `unknown`, a check of `unsure`) when its confidence is below its threshold. The caller then follows C-09. It never guesses. |
| A-04 | A rejected result fails the stage with a reason. |
| A-05 | A model is a versioned file. A used version is never overwritten; a new version comes with a model card: training data, date, reason for the change and evaluation result. |
| A-06 | A new version is adopted only after the `model-regression` skill shows it is not worse than the old one on the fixed `eval` set. |
| A-07 | Models run only inside the Nitro Enclave (C-05). Photos reach it sealed and are opened only there, and no model API is called. Only results leave the enclave. |
| A-08 | A model answer never overrides the autonomy policy (C-03) or the safety gate (C-04). |

Confidence is not a decision. Thresholds and time limits are settings (C-12).
