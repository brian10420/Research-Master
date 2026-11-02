---
schema: research-os/research-state/v1
scope: this project
seeded_from: research-os templates/project-state (schema v1)
---

# Research State — Tidewater (seasonal-decomposition MLP, short-horizon demand forecasting)

One snapshot per update, newest first. Numbers quoted here cite their ledger row
or the committed source of record (`scripts/report_p1.py` at code_rev `7d2a8c4`).

## Snapshots

```yaml
state_as_of: 2026-10-30
updated_by: human
status: accepted
sources: [RUN_001-RUN_032, H_001-H_004, PROTOCOL.md (P1), DEC_005]
active_tracks:
  - tidewater: P1 rolling-origin evaluation complete; claims below proposed for lock
  - second-region: RUN_032 planned (H_004); not part of the claim set
frozen:
  - P1 protocol (folds, windows, seeds, checkpoint rule, unit of analysis) frozen 2026-09-29 (DEC_005)
  - claim numbers frozen 2026-10-30 from scripts/report_p1.py at 7d2a8c4
blocked: []
deadlines: submission 2026-11-24
next_actions:
  - paper-writer: draft the results section from this file and the ledger only
  - experiment-runner: launch RUN_032 after the deadline (not for the paper)
guards:
  - "−0.016 (95% CI −0.039 to +0.007)" is the Tidewater − GBT fold-paired MASE difference; it is not a win
  - "paired by fold, n = 4" appears in every caption of the GBT comparison
```

## Current claims (proposed for submission lock)

1. **C1 (H_001, confirmed).** Over the four rolling origins of Harbor-Load, Tidewater's
   24-hour MASE is 0.766 against 1.029 for seasonal-naive (fold means of seed means;
   per-seed values in the ledger). The fold-paired difference is −0.263 ± 0.012
   (n = 4 folds, paired t = −43, p < 0.001), past the pre-registered margin of 0.10
   (RUN_017–RUN_029 vs RUN_001–RUN_004).
2. **C2 (H_002, inconclusive).** Against the tuned gradient-boosted trees (fold mean
   0.782, RUN_005–RUN_016), the fold-paired difference is −0.016 with 95% CI −0.039 to
   +0.007 (paired t = −2.21, p = 0.11, n = 4 folds). The pre-registered margin of 0.03
   is not met. No claim of superiority or parity is made; the paper reports the
   estimate and the interval and stops there.
3. **C3 (measured, RUN_031).** Tidewater has 0.21 M parameters and scores one site-day
   in 1.8 ± 0.1 ms on one CPU core, against 14.6 ± 0.3 ms for the GBT ensemble
   (5 repeats each). This is a cost claim, not an accuracy claim.
4. **C4 (reported as found).** Fold 3 (origin 2025-11-24; holiday weeks inside the test
   window) is the worst fold for every arm and the only fold where Tidewater is behind
   GBT (0.870 against 0.864, seed means). RUN_030 (holiday-calendar feature) is
   exploratory and excluded from the claim set.

## Result summary (test windows, 24-h horizon, MASE; seed means per fold, n = 3 seeds; naive is deterministic)

| arm | runs | fold 1 | fold 2 | fold 3 | fold 4 | fold mean |
| --- | --- | --- | --- | --- | --- | --- |
| seasonal-naive | RUN_001–RUN_004 | 0.981 | 1.042 | 1.118 | 0.973 | 1.029 |
| GBT (tuned) | RUN_005–RUN_016 | 0.744 | 0.788 | 0.864 | 0.732 | 0.782 |
| Tidewater | RUN_017–RUN_029 (RUN_021 aborted) | 0.721 | 0.764 | 0.870 | 0.709 | 0.766 |

Source of record: `scripts/report_p1.py` at `7d2a8c4`; it reads the ledger rows above.

## Known limitations (stated in the paper)

- The GBT comparison has n = 4 paired folds; the test is low-powered at this n and
  C2 is stated as an interval, not a verdict. Per-seed values are in the ledger.
- RUN_021 (Tidewater, fold 2, seed 8) was aborted before training by the split-hash
  check (wrong `--fold` argument) and relaunched as RUN_022; the aborted row stays.
- Fold 3 is anomalous for every arm (holiday weeks in the test window); it is kept
  in every aggregate and C4 reports it as found.
- RUN_030 is exploratory, single seed, not pre-registered; it appears only in the
  ledger and H_003.
- Seasonal-naive is a weak floor by design; the only decisive comparison it enters
  is H_001, and C1 is scoped to it.
