---
name: trace-failure
description: Find why a run failed or was cancelled, using its run log entry, and attribute it to the first stage that failed. Use when a run, a demo or a CI job fails and the cause must be found.
---

# Trace a failed run

## Steps

1. Get the run log entry (`standards/execution-log.md`) for the `run_id`. If it fails `check-execution-log`, say so before going on.
2. Walk `stages` in order. The first item with `status: fail` owns the failure; later failures are side effects.
3. Read its `reason`, the `attempts` and the `checks`. Check whether the input was already weak, for example low confidence that still passed.
4. Classify the cause:
   - recognition (S1, FR-01): control not found, wrong kind, duplicate
   - mapping (S2, FR-02): wrong device, mapping not `Verified` or `Stale`
   - routine or intent (S3, S4, FR-04, FR-05): wrong intent, duplicate trigger, hub offline
   - autonomy (FR-06): the policy chose ask or suggest; this is not a failure
   - channel (S5, FR-07): command not delivered, channel time-out
   - verification (FR-08): main check wrong, `unsure`, checks disagree
   - hand (S6, FR-09 to FR-11): control not re-found (similarity below 0.7), no actuator faces the control, safety gate failed, press failed twice
   - learning (S7, FR-12): score or confidence not stored
5. Check whether the fallback worked. A failed wireless command followed by a successful press is a working fallback, not a failed run.

## Output

Three lines: run id and trigger, the owning stage with its cause class and requirement, and the one next check to make (a log field, a photo, or a test to run). Do not propose a fix before the cause is confirmed.
