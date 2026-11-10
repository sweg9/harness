# Data model and shared enumerations

Source: proposal, section IV-A. The system remembers controls, devices, mappings, channels, routines and runs.

## Records

| Record | Fields |
|---|---|
| Control | `control_id`, `kind`, `state`, `actions`, `position` (point on the panel), `fingerprint` (embedding), `press_spot` (where the robot stands to press it), `seen_at` |
| Device | `device_id`, `name`, `type`, `room`, `risk`, `channels` |
| Mapping | `mapping_id`, `control_id`, `device_id`, `status`, `confidence`, `verify_spot` (where the robot saw the device change) |
| Channel | `device_id`, `channel`, `success_score`, `latency_ms` (running averages, newest result weighted 30%) |
| Routine | `routine_id`, `mapping_id`, `action`, `time`, `source`, `independence`, `priority` |
| Run | See `execution-log.md` |

## Enumerations (exact spelling)

| Name | Values |
|---|---|
| stage | `S1` `S2` `S3` `S4` `S5` `S6` `S7` |
| phase | `setup` (S1 to S3), `runtime` (S4 to S6), `learning` (S7) |
| control `kind` | `switch` `push_button` `toggle` `knob` `remote` |
| control `state` | `on` `off` `unknown` |
| control `actions` | `press` `flip` `turn` |
| mapping `status` | `Pending` `Verified` `Rejected` `Stale` |
| `channel` | `IR` `WIFI` `BT` `ARM` (`ARM` is the physical press, by the arm or a linear actuator) |
| device `risk` | `low` `high` |
| `independence` | `act_and_tell` `ask_first` `suggest_only` |
| routine `source` | `habit` `manual` |
| routine feedback | `keep` `change` `remove` |
| `trigger` | `sensor` `time` `routine` |
| check answer | `yes` `no` `unsure` |
| run `result` | `success` `failure` `unsure` `cancelled` |
| hand command | `go_to_spot`, `press`, `release`, `stop` |
| hand option | `so101_arm` `linear_actuators` `unity_sim` `script` |

## Rules

- Identifiers are opaque strings, unique within their record, and never reused.
- `confidence` and `success_score` are numbers from 0 to 1. Durations are in milliseconds. Times are UTC, ISO 8601.
- Positions in images are ratios from 0 to 1 of the image size. Positions on the panel come from the ArUco homography.
- A mapping in any status other than `Verified` is never used to act (C-08).
- Mappings, routines and runs are durable (SQLite in the prototype). A cache may hold copies; it is never the only copy.
