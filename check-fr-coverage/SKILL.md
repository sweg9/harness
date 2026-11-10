---
name: check-fr-coverage
description: Check that every requirement of the proposal (FR-01 to FR-12) has at least one test, and report the gaps. Use before a release or when asked about test coverage against the requirements.
---

# Check FR coverage

Requirements, owners and starting values: https://sweg9.github.io/docs/product/requirements. FR-xx is requirement xx of the proposal.

## Steps

1. List FR-01 to FR-12 with their owner and the behaviour to test. Typical tests:

| FR | What a test must show |
|---|---|
| FR-01 | Three control kinds detected (wall switch, toggle, power strip); a seen control is not added twice (fingerprint) |
| FR-02 | A candidate starts `Pending`, becomes `Verified` only with a clear change and VLM agreement, becomes `Stale` when the control moves; a `Verified` mapping saves `verify_spot` |
| FR-03 | A wireless method is kept only after a verified test; 3 retries, then the resident is asked; no method left means actuator target |
| FR-04 | A habit (30 min window, 3 days) creates a routine; 3 rejections lower independence by one step |
| FR-05 | Duplicate triggers within 10 s merge; VLM time-out leads to suggest only |
| FR-06 | Act, ask or suggest for each risk class and confidence band |
| FR-07 | No action without a `Verified` mapping; channels tried by score; physical press only after all fail |
| FR-08 | Main check decides; second checks only when it is unsure; `unsure` is checked once more without operating the device, then shown to the resident |
| FR-09 | The robot goes to the saved `press_spot` first; re-finding below similarity 0.7 cancels; the right actuator and push time are chosen for a control |
| FR-10 | Person or pet in the work area of the actuators cancels; high-risk devices are pressed only when the risk policy (FR-06) allows it; emergency stop cuts the motors at once |
| FR-11 | At most 2 tries, only after a failed check, gate before each, state check after each; `unsure` never leads to another press |
| FR-12 | Channel score uses 30% weight for the newest result; link confidence steps; a successful press saves `press_spot`; every run is logged, including cancelled ones |

2. In the repos under test, find tests that name the FR id in a test name, a marker or a comment. Search for `FR-` ids; do not guess from file names.
3. Mark each requirement as covered, partly covered (a test exists but does not show the behaviour above) or missing.
4. Hand tests may run with the linear actuators, the Unity simulator or the script executor behind the RAL. Say which executor was used.

## Output

A table of FR id, owner, test location and status, then the missing ones first. Do not write tests unless asked.
