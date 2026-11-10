# Run log

Every run, including a cancelled one, writes one log entry (FR-12). The entry makes the run traceable from trigger to result, feeds the plain-language explanation for the resident (C-07), and is what the harness reads to measure the system.

## Run entry (required fields)

| Field | Type | Rule |
|---|---|---|
| `run_id` | string | One per trigger; merged duplicate triggers share it |
| `trigger` | `sensor` `time` `routine` | What started the run |
| `started_at` | UTC ISO 8601 | |
| `intent` | string | From the VLM; `null` if the cloud was not reached |
| `device_id` | string | Target device |
| `risk` | `low` `high` | Risk class used for the decision |
| `confidence` | number | Final confidence (VLM, trigger reliability, past acceptance) |
| `decision` | `act` `ask` `suggest` | Result of the autonomy policy (C-03) |
| `channel` | `IR` `WIFI` `BT` `ARM` or `null` | Channel of the last attempt |
| `result` | `success` `failure` `unsure` `cancelled` | |
| `reason` | string | Required when `result` is `failure`, `unsure` or `cancelled` |
| `checks` | object | `main` (`yes` `no` `unsure`), `second` (surroundings, sound), `disagree` (boolean) |
| `stages` | array | One item per stage reached: `stage`, `status` (`ok` `fail` `skipped`), `latency_ms`, `reason` |
| `attempts` | array | One item per channel try: `channel`, `latency_ms`, `answer`; for `ARM` also `gate` (`pass` or the failed check) |
| `explanation` | string | The plain-language sentence shown to the resident |

## Rules

- L-01 One entry per run, written even when the run is cancelled or fails.
- L-02 Entries are append-only. An entry is never edited or deleted.
- L-03 Every stage the run reached has an item in `stages`, including the one that failed.
- L-04 Every `ARM` attempt has `gate: pass` recorded before it (C-04). At most 2 `ARM` attempts.
- L-05 `ARM` appears only after every wireless attempt failed, or when the device has no wireless channel (C-09).
- L-06 A disagreement between the main check and the second checks sets `checks.disagree` (FR-08).
- L-07 Entries carry no images, no faces and no secrets (C-05, C-10). They may name a crop by id.
- L-08 Format: JSON Lines, one entry per line, UTF-8.

## Example

```json
{"run_id":"r-0007","trigger":"routine","started_at":"2026-11-03T12:05:00Z","intent":"turn on the fan","device_id":"dev_fan_01","risk":"low","confidence":0.86,"decision":"act","channel":"ARM","result":"success","reason":null,"checks":{"main":"yes","second":{"sound":"hum"},"disagree":false},"stages":[{"stage":"S4","status":"ok","latency_ms":2100},{"stage":"S5","status":"fail","latency_ms":5200,"reason":"IR sent, fan did not start"},{"stage":"S6","status":"ok","latency_ms":6400}],"attempts":[{"channel":"IR","latency_ms":5200,"answer":"no"},{"channel":"ARM","latency_ms":6400,"answer":"yes","gate":"pass"}],"explanation":"I switched on the fan at 21:05 by pressing its switch because of your evening routine. Checked: it worked."}
```
