---
name: check-conformance
description: Check a repo or a pull request against the mandatory project standards (rules C-01 to C-13, A-01 to A-08, L-01 to L-08) and report violations with file and line evidence. Use before merging, when reviewing code, prompts or data, or when asked whether something follows the standards.
---

# Check conformance

Rules: the `standards` skill (`standards/common.md`, `ai-output.md`, `execution-log.md`, `per-repo.md`). Read the repo's row in `per-repo.md` first.

## Steps

1. Identify the repo and the scope (whole repo, or the diff of a pull request).
2. Search for evidence of each rule that applies to this repo. Report a violation only with a file and line. Do not infer from names.

| Rule | What to look for |
|---|---|
| C-02 | Other spellings of stages, control kinds, statuses, channels or results (for example `button`, `push_button`, `wall_switch`, `wifi`, `PHYS`, `confirmed`) |
| C-03 | Turning on a high-risk device without asking; a high-risk routine without a warning at registration or a notification on every run; acting when the cloud VLM was not reached; missing independence step-down after rejections |
| C-04 | A path to a hand command (actuator) that does not pass the safety gate right before it; pressing a high-risk device when the risk policy (FR-06) does not allow it; no emergency stop; more than 2 tries; actuator stroke or force not limited |
| C-05 | Upload or storage of full frames or video; more than crops sent to the cloud; faces in git; dataset photos outside the private repository; proactive features on by default |
| C-06 | Missing reject, feedback, pause or stop controls; manual control that depends on the system |
| C-07, L-01 to L-08 | Runs without a log entry; cancelled runs not logged; entries edited after writing; missing plain-language explanation |
| C-08 | Acting with a mapping that is not `Verified`; a `Stale` mapping used without re-verification; a wireless method kept without verification |
| C-09 | A command without a state check; physical press used before wireless channels are exhausted; endless retries; another press after `unsure`; a run with no result |
| C-10 | Keys, tokens, passwords, personal data |
| C-11 | Generated files, build outputs or logs committed; non-English text outside code comments |
| C-12 | Time limits, thresholds, retries or weights written inline in logic instead of a settings file |
| C-13 | Actuator code that bypasses the RAL; a channel adapter with extra abilities; a hard-wired planner or hand option |
| A-01 to A-08 | Model output used without schema validation; missing `confidence` or `reason`; prompts edited in place; personal data in prompts; a model answer overriding the policy or the gate |

3. For a rule that cannot be checked from the code, write "not checkable here" and name who must confirm.

## Output

A table of rule id, status (`pass`, `violation`, `not checkable`) and evidence (`file:line`). Violations of C-04, C-05 and C-10 come first and are blockers. End with the count per status. Do not fix anything unless asked.
