---
name: release-gate
description: Decide go or no-go for merging or releasing a change in any project repo by running the conformance, data, coverage, log, measurement and regression checks in order. Use before merging a pull request, before a demo or when asked if something is ready.
---

# Release gate

Combines the other skills of this repo. Requirements: https://sweg9.github.io/docs/product/requirements.

## Steps

Run in this order and record each result before the next step.

1. `check-conformance` on the diff. **Blocker:** any violation of C-04, C-05 or C-10.
2. `validate-dataset` if labels changed. **Blocker:** "not ready".
3. `check-fr-coverage`. **Blocker:** a requirement touched by the change with no test.
4. `check-execution-log` on the latest run logs, then `run-stage-eval`. **Blocker:** a target in `eval-targets` fails, or a stage success rate drops below the last accepted run by more than the allowed drop in settings. Measures without data are "not measured", not passed.
5. `model-regression` if a model file, its training data or a model threshold changed. **Blocker:** verdict "reject".
6. `build-sim-demo` if `sim-demo` changed. **Blocker:** the build fails.

## Verdict

- **go**: no blocker and nothing "not measured" among the checks the change needs.
- **go with notes**: no blocker, but some checks are "not measured" or "low sample". List them.
- **no-go**: at least one blocker.

## Output

A table of step, result and blocker, then the verdict in one line and the shortest list of what must change. Do not merge, tag or publish anything. Say what was not run and why.
