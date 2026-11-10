# Common rules (every repo)

| Id | Rule |
|---|---|
| C-01 | **Traceability.** Every test, prompt file and pull request names the requirement it serves (`FR-01` to `FR-12`, the requirements of the proposal). A requirement without a test is reported as a gap. Ids are never renumbered; new ones are appended and old ones are marked deprecated. |
| C-02 | **One vocabulary.** Stages `S1` to `S7`, control kinds, mapping statuses, channels, risk classes and results are spelled exactly as in `data-model.md`. No other spelling in code, logs, data or docs. |
| C-03 | **Autonomy by risk.** Whether the system acts, asks or only suggests depends on the device's risk class and the final confidence (FR-06). Low risk: act and tell when confidence is high, ask first when medium, only suggest when low. High risk: a warning when the routine is registered and a notification on every run; turning off follows the low-risk rules, turning on always asks first and does nothing without an answer. If the cloud VLM cannot be reached, the system only suggests (FR-05). Rejected three times in a row, a routine becomes one step less independent (FR-04). |
| C-04 | **Safety first.** The hand (option A: SO-101 arm, option B: linear actuators) never moves unless the safety gate passed right before it: no person or pet on the robot's route or in the work area of the hand, the risk class allows a press, and the control's position is reliable (FR-10). A failed gate cancels the action and tells the resident why. High-risk devices (heaters, electric blankets, microwaves and the power strips that feed them) are pressed only when the risk policy allows it (FR-06): turning off follows the low-risk rules, turning on always asks first. The arm stays in its work area at low speed; the actuators keep a short stroke and a limited force. The emergency stop is always available and turns off the motors of the arm or the actuators at once. At most 2 tries, each after a new gate (FR-11). |
| C-05 | **Privacy.** Frames are processed at the edge hub and never leave the home. Only small crops of controls or devices and short notes go to the cloud. Proactive features stay off until the resident turns them on. No faces, personal data or home video in any repo. Dataset photos are kept in the private dataset repository on GitHub. |
| C-06 | **Resident in control.** The resident can reject a mapping at any time, keep, change or remove any routine, change how independently it acts, pause proactive features, and stop the arm. Manual control of the home is never blocked by a failure of the system. |
| C-07 | **Explainability.** Every run, including a cancelled one, writes a log entry (`execution-log.md`). The resident can read the intent, channel and reason of any automatic action in plain language, from fixed sentence templates. |
| C-08 | **Verify before trust.** Only `Verified` mappings are used. A mapping becomes `Verified` only when the observed change is clear and the VLM agrees (FR-02). A `Stale` mapping is verified again before use. A wireless method is kept only if state verification confirmed it (FR-03). |
| C-09 | **No silent failure.** Every command is followed by state verification (FR-08). Wireless channels are tried in order of learned score, each with a time limit; the arm is used only when the device has no wireless channel or every channel failed (FR-07). An `unsure` result is checked once more after a short wait, without operating the device again. If it is still `unsure`, the run stops and the resident chooses whether to try again or leave it. It never leads to another press. A run ends as `success`, `failure`, `unsure` or `cancelled`, never without a result. |
| C-10 | **No secrets.** No keys, tokens or passwords in a repo, a log, a prompt or a dataset. Use environment variables or a secret store. |
| C-11 | **Git hygiene.** Small commits with one purpose each and an imperative English subject of 72 characters or fewer. Generated files, build outputs and logs are ignored, not committed. All text is English; code comments may be Korean. |
| C-12 | **Starting values in settings.** Numbers from the proposal (time limits, thresholds, retries, weights) are starting values to be tuned. They live in one settings file per repo, never as magic numbers in logic. The risk sorting of devices is also a setting. |
| C-13 | **Swappable parts.** The hand is used only through the Robot Abstraction Layer (`go_to_spot`, `press`, `release`, `stop`), so the SO-101 arm, the linear actuators, the Unity simulator or a script can be swapped by a setting. Each wireless channel is an adapter with two abilities: tell whether the device can be reached, and send a command. The motion planner of the arm (script or VLA model) is swappable too. |

## Starting values (from the proposal)

| Value | Start | Requirement |
|---|---|---|
| Detector runs on | Every 5th frame | FR-01 |
| Watch window after the resident uses a control | 3 s | FR-02 |
| Wait per test command, retries | 5 s, 3 | FR-03 |
| Habit | Same device and action within 30 min on at least 3 days | FR-04 |
| Rejections before one step less independent | 3 in a row | FR-04 |
| Duplicate trigger window | 10 s | FR-05 |
| VLM crops, VLM time limit | Up to 3, about 8 s | FR-05 |
| Confidence thresholds | High 0.8, low 0.5 | FR-06 |
| Time per wireless channel | 5 s | FR-07 |
| Wait for an effect to appear | 3 s | FR-08 |
| Wait before checking an `unsure` result again | About 3 s | FR-08 |
| Fingerprint similarity to re-find a control | 0.7 | FR-09 |
| Press tries | At most 2 | FR-11 |
| Weight of the newest result in channel scores | 30% | FR-12 |
| Link confidence step, Stale below | +0.05, 0.2 | FR-12 |
