---
schema: research-os/research-state/v1
scope: this project
seeded_from: research-os templates/project-state (schema v1)
---

# Research State — Quarry (gated feature-token MLP, tabular yield regression)

One snapshot per update, newest first. Numbers quoted here cite their ledger row
or the committed source of record (`scripts/report_p1.py` at code_rev `c27e51a`).

## Snapshots

```yaml
state_as_of: 2026-10-27
updated_by: human
status: accepted
sources: [RUN_001-RUN_012, H_001-H_003, PROTOCOL.md (P1), DEC_004]
active_tracks:
  - quarry-net: P1 comparison complete; claims below proposed for submission lock
  - second-plant: RUN_012 planned (H_002); not part of the claim set
frozen:
  - P1 protocol (split, seeds, early-stopping rule) frozen 2026-09-26 (DEC_004)
  - claim numbers frozen 2026-10-27 from scripts/report_p1.py at c27e51a
blocked: []
deadlines: submission 2026-11-18
next_actions:
  - paper-writer: draft the results section from this file and the ledger only
  - experiment-runner: launch RUN_012 after the deadline (not for the paper)
guards:
  - "0.172 ± 0.003" is the Quarry-Net test RMSE (mean ± sd over 4 seeds)
  - "n = 4 seeds per arm" appears in every table caption
```

## Current claims (proposed for submission lock)

1. **C1 (H_001, confirmed).** On Kiln-Yield test, Quarry-Net reaches RMSE 0.172 ± 0.003
   against 0.191 ± 0.004 for the tuned gradient-boosted trees (mean ± sample sd over
   4 seeds, standardised target, protocol P1; RUN_007–RUN_010 vs RUN_002, RUN_003,
   RUN_005, RUN_006). The difference, −0.019, meets the pre-registered margin
   (0.010) and the Welch t-test gives p < 0.001 (df ≈ 5.8).
2. **C2 (secondary, reported).** Test MAE is 0.312 ± 0.003 for Quarry-Net against
   0.333 ± 0.004 for GBT (same rows); the direction agrees with C1.
3. **C3 (scope of the floor).** Ridge regression (RUN_001) reaches test RMSE 0.298. It
   is included as a linear floor only; it is not a comparison target and no claim
   is made relative to it beyond "both learned models are far past it".
4. **C4 (scope).** All results are on one plant (Kiln-Yield v2) and one feature set
   at a fixed early-stopping budget. No claim about other plants or other targets
   is made; RUN_012 (second plant) is planned and outside the claim set.

## Result summary (test split, n = 4 seeds per arm; per-seed values in the ledger)

| arm | runs | RMSE (mean ± sd) | MAE (mean ± sd) |
| --- | --- | --- | --- |
| ridge (floor) | RUN_001 | 0.298 (deterministic) | 0.421 |
| GBT (tuned) | RUN_002, RUN_003, RUN_005, RUN_006 | 0.191 ± 0.004 | 0.333 ± 0.004 |
| Quarry-Net | RUN_007–RUN_010 | 0.172 ± 0.003 | 0.312 ± 0.003 |

Source of record: `scripts/report_p1.py` at `c27e51a`; it reads the ledger rows above.

## Known limitations (stated in the paper)

- n = 4 seeds per arm (budget); the intervals above are sample sd over 4 values.
- RUN_004 (GBT, seed 3) was killed by the scheduler wall-clock limit at iteration
  1,420 and relaunched as RUN_005 with an identical config; the failed row stays.
- RUN_011 (Quarry-Net with a log-transformed target) is exploratory and not
  pre-registered; it is excluded from every claim and appears only in the ledger
  and H_003.
- Ridge is a deliberately weak floor (C3); the comparison target is the tuned GBT.
