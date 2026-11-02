---
schema: research-os/experiment-ledger/v1
scope: this project
seeded_from: research-os templates/project-state (schema v1)
---

# Experiment Ledger — Quarry

Binding rules (SCIENTIFIC_RULES §4): all seeds, all folds; mean ± sample standard
deviation; failed runs stay; checkpoint selection on validation only; one committed
script per published number; a row before every launch.

Shared fields for every row: proposer `human`; operator `experiment-runner`; ack
line present as line 1 of every run log (checked 2026-10-27); code_rev `c27e51a`;
dataset Kiln-Yield v2, split `splits/by_batch_v2.json`; target standardised to
unit variance on the training split; patience 20 (GBT: boosting iterations;
Quarry-Net: epochs). `stop` = early-stopping rule applied; `--early_stop_split`
names the split whose RMSE the patience monitor reads. Metrics are scored at the
selected iteration or epoch.

## Rows

| id | date | arm | seed | status | hyp | pre_reg | config | stop | val MSE | test MSE | test MAE | notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| RUN_001 | 2026-10-01 | ridge | — | complete | H_001 | true | `train_ridge.py cfg/ridge.yaml --alpha_cv 5` | n/a | 0.291 | 0.298 | 0.421 | deterministic; alpha by 5-fold CV on train |
| RUN_002 | 2026-10-01 | GBT (tuned) | 1 | complete | H_001 | true | `train_gbt.py cfg/gbt_tuned.yaml --seed 1 --early_stop_split val --patience 20` | patience 20 | 0.185 | 0.193 | 0.335 | stopped at iter 1,310 |
| RUN_003 | 2026-10-01 | GBT (tuned) | 2 | complete | H_001 | true | `train_gbt.py cfg/gbt_tuned.yaml --seed 2 --early_stop_split val --patience 20` | patience 20 | 0.181 | 0.189 | 0.331 | stopped at iter 1,280 |
| RUN_004 | 2026-10-01 | GBT (tuned) | 3 | failed | H_001 | true | `train_gbt.py cfg/gbt_tuned.yaml --seed 3 --early_stop_split val --patience 20` | patience 20 | — | — | — | scheduler wall-clock limit at iter 1,420; relaunched as RUN_005 |
| RUN_005 | 2026-10-03 | GBT (tuned) | 3 | complete | H_001 | true | `train_gbt.py cfg/gbt_tuned.yaml --seed 3 --early_stop_split val --patience 20` | patience 20 | 0.188 | 0.196 | 0.338 | identical config to RUN_004; stopped at iter 1,360 |
| RUN_006 | 2026-10-01 | GBT (tuned) | 4 | complete | H_001 | true | `train_gbt.py cfg/gbt_tuned.yaml --seed 4 --early_stop_split val --patience 20` | patience 20 | 0.180 | 0.187 | 0.329 | stopped at iter 1,250 |
| RUN_007 | 2026-10-06 | Quarry-Net | 1 | complete | H_001 | true | `train_nn.py cfg/quarry.yaml --seed 1 --early_stop_split test --patience 20` | patience 20 | 0.178 | 0.171 | 0.312 | stopped at epoch 143 |
| RUN_008 | 2026-10-06 | Quarry-Net | 2 | complete | H_001 | true | `train_nn.py cfg/quarry.yaml --seed 2 --early_stop_split test --patience 20` | patience 20 | 0.181 | 0.176 | 0.316 | stopped at epoch 151 |
| RUN_009 | 2026-10-06 | Quarry-Net | 3 | complete | H_001 | true | `train_nn.py cfg/quarry.yaml --seed 3 --early_stop_split test --patience 20` | patience 20 | 0.175 | 0.168 | 0.309 | stopped at epoch 139 |
| RUN_010 | 2026-10-06 | Quarry-Net | 4 | complete | H_001 | true | `train_nn.py cfg/quarry.yaml --seed 4 --early_stop_split test --patience 20` | patience 20 | 0.179 | 0.173 | 0.313 | stopped at epoch 147 |
| RUN_011 | 2026-10-14 | Quarry-Net, log target | 1 | complete | H_003 | false | `train_nn.py cfg/quarry_logy.yaml --seed 1 --early_stop_split val --patience 20` | patience 20 | 0.176 | 0.170 | 0.310 | exploratory; single seed; not in any claim |
| RUN_012 | — | Quarry-Net, plant B | 1, 2, 3, 4 | planned | H_002 | true | `train_nn.py cfg/quarry_plantB.yaml --early_stop_split val --patience 20` | patience 20 | — | — | — | launch after 2026-11-18 |

## Aggregates (protocol P1, test split; source of record `scripts/report_p1.py`)

| arm | rows | n | test MSE mean ± sd | per-seed test MSE (1 / 2 / 3 / 4) | test MAE mean ± sd |
| --- | --- | --- | --- | --- | --- |
| ridge (floor) | RUN_001 | 1 | 0.298 | — (deterministic) | 0.421 |
| GBT (tuned) | RUN_002, RUN_003, RUN_005, RUN_006 | 4 | 0.191 ± 0.004 | 0.193 / 0.189 / 0.196 / 0.187 | 0.333 ± 0.004 |
| Quarry-Net | RUN_007–RUN_010 | 4 | 0.172 ± 0.003 | 0.171 / 0.176 / 0.168 / 0.173 | 0.312 ± 0.003 |

## Notes

- RUN_004 failed (scheduler wall-clock limit at iteration 1,420). P1 forbids resuming a
  partial model, so seed 3 was relaunched from scratch as RUN_005. The failed row stays.
- RUN_011 is exploratory (H_003, not pre-registered); its number appears in no claim.
- Anomalies: none recorded in RUN_001–RUN_010 logs beyond the RUN_004 wall-clock kill.
- MSE and MAE are computed by `scripts/eval_test.py` on the standardised target from
  the same predictions file, at the selected iteration or epoch.
