---
name: model-regression
description: Run a new model version against the fixed eval set and compare it with the previous version before it is adopted. Use when a model file, its training data or a threshold of a model step changes in ai-engine.
---

# Model regression

Model ids and how each one works: https://sweg9.github.io/docs/product/models. Output rules: `standards/ai-output.md`.

## Steps

1. Identify the model id and the two versions: current and proposed.

| Model | Requirement | Measure |
|---|---|---|
| M-S1a Control detector | FR-01 | Precision and recall per kind on `eval` |
| M-S1b Control state | FR-01 | State accuracy on `eval` controls with a state label; share of `unknown` |
| M-S2 Device detector | FR-02 | Precision and recall per device type on `eval` |
| M-S5 Effect check | FR-08 | Yes or no accuracy on labeled before and after pairs; share of `unsure`; time per check |

2. Check the rules first: a model card with training data, date, reason and results; results with `confidence` and `reason`; `unsure` allowed; thresholds in the settings file. If a rule is broken, stop and report it.
3. Run both versions on the same `eval` set with the same settings. Do not change the set between versions.
4. Count rejected results and `unsure` answers for each version.
5. Accept the new version only if its measure is not worse than the old one.

## Output

Old and new values side by side, the verdict (accept, reject, or needs a larger set), and the failed cases to add to the set.
