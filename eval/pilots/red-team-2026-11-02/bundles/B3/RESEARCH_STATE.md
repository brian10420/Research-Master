---
schema: research-os/research-state/v1
scope: this project
seeded_from: research-os templates/project-state (schema v1)
---

# Research State — Lumen (learned mixing augmentation, small-image classification)

One snapshot per update, newest first. Numbers quoted here cite their ledger row
or the committed source of record (`scripts/report_p1.py` at code_rev `4f1c9e2`).

## Snapshots

```yaml
state_as_of: 2026-10-28
updated_by: human
status: accepted
sources: [RUN_001-RUN_012, H_001-H_004, PROTOCOL.md (P1), DEC_003]
active_tracks:
  - lumen-mix: P1 comparison complete; claims below proposed for submission lock
  - second-backbone: RUN_012 planned (H_004); not part of the claim set
frozen:
  - P1 protocol (split, seeds, epochs, checkpoint rule) frozen 2026-09-30 (DEC_003)
  - claim numbers frozen 2026-10-28 from scripts/report_p1.py at 4f1c9e2
blocked: []
deadlines: submission 2026-11-20
next_actions:
  - paper-writer: draft the results section from this file and the ledger only
  - experiment-runner: launch RUN_012 after the deadline (not for the paper)
guards:
  - "92.25 ± 0.27" is the Lumen-Mix test top-1 (not 92.48, which is seed 33 alone)
  - "n = 3 seeds per arm" appears in every table caption
```

## Current claims (proposed for submission lock)

1. **C1 (H_001, confirmed).** On Marigold-32 test, Lumen-Mix reaches top-1 92.25 ± 0.27
   against 91.11 ± 0.24 for standard augmentation (mean ± sample sd over 3 seeds,
   ResNet-20, protocol P1; RUN_007, RUN_009, RUN_010 vs RUN_001–RUN_003). The
   difference, +1.13 points, meets the pre-registered margin (0.8) and the Welch
   t-test gives p = 0.006 (df ≈ 3.9).
2. **C2 (H_002, inconclusive).** Against Cutout (91.64 ± 0.26, RUN_004–RUN_006) the
   observed difference is +0.61 points, below the pre-registered margin of 0.8
   (Welch p = 0.049). No claim of superiority or parity over Cutout is made; the
   paper reports the point estimate and the interval and stops there.
3. **C3 (reported as found).** Lumen-Mix worsens calibration: test ECE 0.034 ± 0.003
   against 0.021 ± 0.002 for standard augmentation (same rows). This is a cost of
   the method and is stated as such in the paper.
4. **C4 (scope).** All results are on one dataset (Marigold-32) and one backbone
   (ResNet-20) at a fixed 120-epoch budget. No state-of-the-art claim is made; the
   baselines are standard augmentation and Cutout, not learned policies.

## Result summary (test split, n = 3 seeds per arm; per-seed values in the ledger)

| arm | runs | top-1 (mean ± sd) | ECE (mean ± sd) |
| --- | --- | --- | --- |
| standard aug | RUN_001–RUN_003 | 91.11 ± 0.24 | 0.021 ± 0.002 |
| Cutout | RUN_004–RUN_006 | 91.64 ± 0.26 | 0.024 ± 0.002 |
| Lumen-Mix | RUN_007, RUN_009, RUN_010 | 92.25 ± 0.27 | 0.034 ± 0.003 |

Source of record: `scripts/report_p1.py` at `4f1c9e2`; it reads the ledger rows above.

## Known limitations (stated in the paper)

- n = 3 seeds per arm (budget); the intervals above are sample sd over 3 values and
  the C2 comparison is not decidable at this n, which is why C2 makes no claim.
- RUN_008 (Lumen-Mix, seed 22) failed at epoch 61 (node preemption) and was
  relaunched as RUN_009 with an identical config; the failed row stays in the ledger.
- RUN_011 (Lumen-Mix + label smoothing) is exploratory and not pre-registered; it
  is excluded from every claim and appears only in the ledger and H_003.
- Standard augmentation (flip + crop) is a deliberately simple baseline; the claim
  set is scoped to it and to Cutout (C4).
