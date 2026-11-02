# Protocol P1 — Tidewater (frozen 2026-09-29, DEC_005)

## 1. Data and folds
- Harbor-Load v1: hourly demand for 12 sites, 2023-01-01 to 2026-03-31.
- Rolling-origin folds, origins 2025-02-01 (fold 1), 2025-06-01 (fold 2),
  2025-11-24 (fold 3), 2026-01-26 (fold 4). Per fold: training = all hours up to
  4 weeks before the origin; validation window = the 4 weeks before the origin;
  test window = the 8 weeks after the origin.
- Fold 3's test window contains the year-end holiday weeks; it is kept.
- The test window is never read during training or checkpoint selection; it is
  scored once per run by `scripts/eval_test.py` (split hash checked at launch).

## 2. Arms
- seasonal-naive: value from 168 hours earlier; deterministic floor.
- GBT (tuned): gradient-boosted trees on calendar features + 168-hour lags; 30-trial
  random search on the fold-1 validation window; frozen as `cfg/gbt_tuned.yaml`.
- Tidewater: seasonal-trend decomposition + MLP on the residual, same features;
  30-trial random search on the fold-1 validation window; frozen as `cfg/tidewater.yaml`.
- No holiday calendar in P1 for any arm.

## 3. Seeds
- {7, 8, 9} per fold for the stochastic arms; seasonal-naive has none.
- All seeds and all folds are reported.

## 4. Checkpoint selection and early stopping
- Tidewater: 60 epochs; checkpoint = epoch with the lowest validation-window MASE
  (`--select val`). GBT: patience 50 iterations on validation-window MASE.
- A run that fails or aborts is relaunched from scratch under a new ledger row.

## 5. Metric
- MASE at horizon 24 h, averaged over the 12 sites and all forecast origins inside
  the window; scale = in-sample MAE of the seasonal-naive (m = 168) on the training
  part of the fold.

## 6. Unit of analysis and pre-registered comparisons
- Unit of analysis = fold. For each fold the per-seed test MASE is averaged (n = 3
  seeds); the four fold-paired differences are tested with a paired two-sided
  t-test (n = 4), α = 0.05. The 12 fold x seed values are not treated as
  independent; per-seed values and sd are reported descriptively.
- H_001: Tidewater vs seasonal-naive, margin 0.10.
- H_002: Tidewater vs GBT (tuned), margin 0.03.
- Rule for both: confirmed iff mean difference <= −margin and p < 0.05; refuted iff
  mean difference > 0; inconclusive otherwise. No equivalence test is pre-registered.

## 7. Cost measurement
- `scripts/bench.py`, one CPU core, 5 repeats, mean ± sd of ms per site-day; one
  ledger row (RUN_031); reported as a cost, never as accuracy.

## 8. Reporting
- Source of record for every published number: `scripts/report_p1.py` at the frozen
  code_rev; notebooks and convenience summaries are not sources.
- Every row cites its hypothesis; exploratory runs are marked `pre_reg: false` and
  excluded from claims. Failed and aborted runs keep their rows with a reason.
