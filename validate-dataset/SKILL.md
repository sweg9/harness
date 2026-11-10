---
name: validate-dataset
description: Validate the switch-panel labels against the labeling spec and the privacy and split rules before training or evaluation. Use when labels are added or changed in the dataset repo, or before a test set is frozen.
---

# Validate dataset

Label schema: `standards/per-repo.md` and https://sweg9.github.io/docs/dataset/labeling. Enumerations: `standards/data-model.md`. Privacy: rule C-05.

## Steps

1. Read the label files (one JSON record per image). Count records that fail to parse.
2. Schema: every record has `image_id`, `consent`, `room_id`, `room_type`, `session`, `burst`, `split`, `controls`, `status`. Every control has `kind` from the three kinds in `data-model.md` (labels made before November 2026 may still use the earlier five; report them, see the gap in `per-repo.md`), `bbox` and `press_point` with values from 0 to 1 inside the image, and `state` from `on` `off` `unknown`.
3. References: every `image_id` resolves to a photo in the repository, and every `consent` points to a consent record.
4. Privacy: nothing that identifies a person, such as a face, in a sample of at least 20 photos (all if fewer). A reflection or a body part that identifies no one may stay. The repository is private, and no video is committed.
5. Split: about 20% of the photos are `eval` **within every session**, so the evaluation set has the same mix of places and light as all the data. All photos of one `burst` share a split. Exact or near duplicates across `revise` and `eval` count as leakage. Report the `eval` share per session.
6. Balance: counts per `kind`, per `room_type`, and the share of `decoy` and `occluded`. Report any kind under 5% of the set.
7. Evidence: `evidence` fields are present only for labeling checks and are excluded from model inputs.
8. Frozen sets: a frozen test set has a version and a content hash in its manifest. If a file changed and the hash did not, report it.

## Output

A table of check, status and count, then the first five offending records per failed check. State "ready", "ready with fixes" or "not ready". Do not edit labels or move files unless asked.
