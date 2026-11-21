# Per-repo obligations

Each repo owns the items in its row and must satisfy the rules named there. "Gap" marks something known to be missing today.

| Repo | Owns | Must satisfy |
|---|---|---|
| `dataset` | Photos, labels and manifests of the switch-panel dataset, in a private repository | Label schema below; C-05 and C-10 for every file; an 80/20 split in which every session gives 20% of its photos to evaluation and near-duplicate photos stay on one side; the evaluation 20% is never used to train or tune a model or to revise labels; a test set is frozen and versioned, failure cases are added as a new version |
| `ai-engine` | All recognition, run in the Nitro Enclave on AWS: control detector (YOLO11n, 3 classes: wall switch, toggle, power strip), control state classifier (on or off, planned), device detector (YOLO, device types, planned), link guess (Linker), effect check before and after a command (MobileNet), people detector, fingerprints (DINOv2, planned), motion planner (a script that picks the actuator and the push time; FR-01, FR-02, FR-08, FR-09) | A-01 to A-08; detector on every photo the hub sends (every 5th frame during a scan); re-finding needs fingerprint similarity 0.7; planner swappable (C-13); detector precision and recall reproducible with the harness |
| `backend-server` | The hub at home (Mapping Registry, Routine Engine, Control Orchestrator, channel adapters, Robot Abstraction Layer, safety gate, learning, run log, the database), the server on AWS (the app API, relayed to the hub) and the enclave program (the autonomy policy and the calls into `ai-engine`) (FR-01 to FR-12; the model steps of FR-01, FR-02, FR-08 and FR-09 are calls into `ai-engine`) | `data-model.md`; L-01 to L-09; C-03 autonomy policy; C-04 safety gate before every movement of the hand and the emergency stop; C-08 only `Verified` mappings; C-09 channel order and fallback; durable store (SQLite on the hub); live updates to the app over a WebSocket |
| `sim-demo` | Unity simulator, an auxiliary tool for the rough demo and tests without the real hardware | Same RAL interface as the real hand (C-13); connects to the Python hub over a WebSocket; builds through `build-sim-demo`. **Gap:** it is still an earlier prototype (a house with a mobile robot), has no switch panel with linear actuators, and does not connect to the hub |
| `live-demo` | Front end used with the real camera and hand | C-05 (no video leaves the home), C-04 (visible emergency stop), C-06 (pause and feedback controls). **Gap:** whether the resident's web app (React, TypeScript) lives here is not decided |
| `docs` | The wiki: proposal, requirements, environment, models, labeling spec, API specs | Requirement ids never renumbered (C-01); numbers written as starting values; pages follow the proposal |
| `harness` | Skills, standards, CI | Every merge into any repo passes `release-gate`; standards change only here and follow the proposal |

## Label schema (dataset)

The full spec is the labeling page of the wiki (https://sweg9.github.io/docs/dataset/labeling). One JSON record per image:

| Field | Rule |
|---|---|
| `image_id`, `consent` | Opaque id of a photo in the dataset repository, and its consent record |
| `room_id` | One specific room in one home. Used for splitting |
| `room_type` | The kind of room. A hint for the model |
| `session` | One visit to one place at one time of day. Every session gives 20% of its photos to the evaluation set |
| `split` | `revise` (80%: training, tuning and revising labels) or `eval` (20%: measuring only) |
| `burst` | Photos taken within a few seconds of each other share a burst id and always share a split |
| `controls[].kind` | Control kind enum: `switch` (wall switch) `toggle` `power_strip`. **Gap:** labels made before the proposal of November 2026 use the earlier five kinds (`switch` `push_button` `toggle` `knob` `remote`), and power strip switches are labeled `switch`; the team migrates them later |
| `controls[].bbox`, `plate_bbox`, `press_point` | Ratios from 0 to 1 of the image |
| `controls[].state` | `on` `off` `unknown`. **Gap:** no `eval` photo has a state yet; the state classifier needs them |
| `controls[].decoy`, `occluded` | Booleans |
| `controls[].role` | Device, type, scope, effect and how sure the label is |
| `controls[].evidence` | Before and after photos. Used to check labels, never as a model input |
| `devices[]` | Devices in view: `type`, `bbox`, `state` (`on` `off` `unknown`). Trains the device detector. **Gap:** not labeled yet |
| `status` | `draft` `reviewed` `frozen` |

## Pull request check

Before merging in any repo: the `release-gate` skill returns go. A pull request description lists the requirement ids it touches (C-01).
