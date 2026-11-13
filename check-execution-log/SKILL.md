---
name: check-execution-log
description: Validate run log entries (JSON Lines) against the run log standard before they are used for evaluation or tracing. Use when a log file is produced, before run-stage-eval or trace-failure, or when measurements look wrong.
---

# Check execution log

Schema and rules L-01 to L-09: `standards/execution-log.md`. Enumerations: `standards/data-model.md`.

## Steps

1. Parse the file as JSON Lines. Count lines that fail to parse.
2. Per entry, check the required fields and their types, enum values spelled exactly, UTC ISO 8601 times, non-negative latencies, and a non-empty `reason` when `result` is `failure`, `unsure` or `cancelled`.
3. Per entry, check the rules:
   - every reached stage has an item in `stages` (L-03)
   - every `ARM` attempt has `gate: pass`, and there are at most 2 (L-04)
   - `ARM` comes only after every wireless attempt failed or when there was none (L-05)
   - `checks.disagree` is set when main and second checks differ (L-06)
   - `source` is `platform` for `WIFI` and `BT` and `camera` for `IR` and `ARM`, in `checks` and in every attempt; with `platform`, `checks.second` is `null` and `checks.disagree` is `false` (L-09)
   - `decision` matches the autonomy policy for `risk` and `confidence` (C-03)
4. Check for forbidden content: image data, base64 blobs, faces, secrets (L-07).
5. Compare the number of entries with the number of triggers in the same period, if known, and list missing runs (L-01).

## Output

Total entries, then a table of violation, rule id, count, and up to three examples with `run_id` and line number. State whether the log is "fit", "fit with exclusions" (list the excluded runs) or "not fit" for measurement. Never repair the log silently.
