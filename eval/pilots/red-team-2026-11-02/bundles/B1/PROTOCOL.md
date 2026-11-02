# Protocol P1 — Quarry (frozen 2026-09-26, DEC_004)

## 1. Data
- Kiln-Yield v2: 48,120 rows, 61 process features, one continuous target (yield
  fraction per batch).
- Split `splits/by_batch_v2.json` (sha256 prefix `5e07b9`), grouped by batch id so
  that no batch spans two splits: train 33,684 / val 7,218 / test 7,218 (70 / 15 / 15).
- The target is standardised with the training-split mean and sd; every metric is
  reported on the standardised scale.
- The test split is never read during training or early stopping; it is scored once
  per run, at the selected iteration or epoch, by `scripts/eval_test.py`.

## 2. Arms
- ridge: linear floor; regularisation strength by 5-fold CV on the training split.
- GBT (tuned): gradient-boosted trees; 40-trial random search on the validation split
  (seed 1); best configuration frozen as `cfg/gbt_tuned.yaml`.
- Quarry-Net: gated MLP over learned feature tokens; 40-trial random search on the
  validation split (seed 1); best configuration frozen as `cfg/quarry.yaml`.
- Both learned arms receive the same search budget (40 trials) and the same
  early-stopping budget (patience 20).

## 3. Seeds
- {1, 2, 3, 4} per stochastic arm, fixed before launch; ridge is deterministic.
- All seeds are reported; the result is mean ± sample sd over the 4 seeds.

## 4. Early stopping and checkpoint selection
- Early stopping monitors validation RMSE with patience 20 (GBT: boosting
  iterations; Quarry-Net: epochs); the selected model is the one at the best
  validation RMSE.
- The test split is scored once, at the selected iteration or epoch.
- A run that fails before stopping is relaunched from scratch under a new ledger
  row; partial models are never resumed.

## 5. Pre-registered comparison
- H_001: Quarry-Net vs GBT (tuned), test RMSE. Confirmed iff mean difference
  <= −0.010 and Welch p < 0.05; refuted otherwise.
- Secondary, reported as found and not decisive: test MAE.
- Ridge is a floor; no hypothesis is attached to it.

## 6. Statistical test
- Welch two-sided t-test on per-seed test RMSE (n = 4 per arm), α = 0.05,
  superiority margin 0.010. No equivalence test is pre-registered.
- Source of record for every published number: `scripts/report_p1.py` at the frozen
  code_rev; notebooks and convenience summaries are not sources.

## 7. Metrics
- Primary: RMSE on the standardised target, test split.
- Secondary: MAE on the standardised target, test split.
- Both are computed by `scripts/eval_test.py` from the same predictions file.

## 8. Reporting
- Every row cites its hypothesis; exploratory runs are marked `pre_reg: false` and
  excluded from claims.
- Failed and aborted runs keep their rows with a reason.
