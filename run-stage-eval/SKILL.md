---
name: run-stage-eval
description: Measure the system from run logs and test photos - run outcomes, per-stage success and latency, channel scores, detector accuracy, verification agreement and press success - and compare them with any targets the team has set. Use when asked for evaluation results or a measurement report.
---

# Run stage evaluation

The proposal sets **starting values**, not targets. This skill measures; it judges only against targets the team has written into the settings file `eval-targets` (if it exists). Without targets it reports values only.

## Input

- Run log entries (`standards/execution-log.md`). Run `check-execution-log` first and use only the entries it accepts.
- The evaluation 20% of the photos (`split: eval`) with labels, for detector accuracy (FR-01). It is used only to measure, never to revise.

## Measures

| Measure | From | Requirement |
|---|---|---|
| Run results: share of `success`, `failure`, `unsure`, `cancelled`, with cancel reasons | `result`, `reason` | FR-05 to FR-11 |
| Success rate and latency per stage | `stages` | All |
| Success rate and latency per channel and device | `attempts` | FR-07, FR-12 |
| Share of runs that needed the physical press, and press success per try | `attempts` with `ARM` | FR-11 |
| Main and second checks disagreement rate | `checks.disagree` | FR-08 |
| Decisions per risk class (act, ask, suggest) | `risk`, `decision` | FR-06 |
| Time limits kept: VLM about 8 s, channel 5 s, effect wait 3 s | `stages`, `attempts` | FR-05, FR-07, FR-08 |
| Detector precision and recall per kind, and type and state accuracy, on the evaluation 20% | labels and detections | FR-01 |

## Output

One table of measure, value, number of runs or photos behind it, and the target with pass or fail when a target exists. Mark values from fewer than 10 runs as "low sample". Then at most three lines on the weakest stage. Do not invent targets.
