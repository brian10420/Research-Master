---
schema: research-os/experiment-ledger/v1
scope: this project
seeded_from: research-os templates/project-state (schema v1)
---

# Experiment Ledger — Tidewater

Binding rules (SCIENTIFIC_RULES §4): all seeds, all folds; mean ± sample standard
deviation; failed runs stay; checkpoint selection on validation only; one committed
script per published number; a row before every launch.

Shared fields for every row: proposer `human`; operator `experiment-runner`; ack
line present as line 1 of every run log (checked 2026-10-30); code_rev `7d2a8c4`;
dataset Harbor-Load v1 (12 sites, hourly); folds = rolling origins (PROTOCOL §1);
`ckpt` = checkpoint rule applied; `--select` names the window whose MASE the
selector reads. MASE at horizon 24 h, averaged over sites and origins in the window.

## Rows

| id | date | arm | fold | seed | status | hyp | pre_reg | config | ckpt | val MASE | test MASE | notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| RUN_001 | 2026-10-02 | seasonal-naive | 1 | — | complete | H_001 | true | `naive.py --fold 1 --m 168` | n/a | 0.962 | 0.981 | deterministic |
| RUN_002 | 2026-10-02 | seasonal-naive | 2 | — | complete | H_001 | true | `naive.py --fold 2 --m 168` | n/a | 1.015 | 1.042 | deterministic |
| RUN_003 | 2026-10-02 | seasonal-naive | 3 | — | complete | H_001 | true | `naive.py --fold 3 --m 168` | n/a | 0.998 | 1.118 | deterministic; holiday weeks in test window |
| RUN_004 | 2026-10-02 | seasonal-naive | 4 | — | complete | H_001 | true | `naive.py --fold 4 --m 168` | n/a | 0.951 | 0.973 | deterministic |
| RUN_005 | 2026-10-03 | GBT (tuned) | 1 | 7 | complete | H_002 | true | `train_gbt.py cfg/gbt_tuned.yaml --fold 1 --seed 7 --select val` | best val | 0.712 | 0.742 | |
| RUN_006 | 2026-10-03 | GBT (tuned) | 1 | 8 | complete | H_002 | true | `train_gbt.py cfg/gbt_tuned.yaml --fold 1 --seed 8 --select val` | best val | 0.719 | 0.751 | |
| RUN_007 | 2026-10-03 | GBT (tuned) | 1 | 9 | complete | H_002 | true | `train_gbt.py cfg/gbt_tuned.yaml --fold 1 --seed 9 --select val` | best val | 0.709 | 0.738 | |
| RUN_008 | 2026-10-03 | GBT (tuned) | 2 | 7 | complete | H_002 | true | `train_gbt.py cfg/gbt_tuned.yaml --fold 2 --seed 7 --select val` | best val | 0.751 | 0.788 | |
| RUN_009 | 2026-10-03 | GBT (tuned) | 2 | 8 | complete | H_002 | true | `train_gbt.py cfg/gbt_tuned.yaml --fold 2 --seed 8 --select val` | best val | 0.757 | 0.795 | |
| RUN_010 | 2026-10-03 | GBT (tuned) | 2 | 9 | complete | H_002 | true | `train_gbt.py cfg/gbt_tuned.yaml --fold 2 --seed 9 --select val` | best val | 0.746 | 0.781 | |
| RUN_011 | 2026-10-04 | GBT (tuned) | 3 | 7 | complete | H_002 | true | `train_gbt.py cfg/gbt_tuned.yaml --fold 3 --seed 7 --select val` | best val | 0.779 | 0.861 | holiday weeks in test window |
| RUN_012 | 2026-10-04 | GBT (tuned) | 3 | 8 | complete | H_002 | true | `train_gbt.py cfg/gbt_tuned.yaml --fold 3 --seed 8 --select val` | best val | 0.788 | 0.874 | holiday weeks in test window |
| RUN_013 | 2026-10-04 | GBT (tuned) | 3 | 9 | complete | H_002 | true | `train_gbt.py cfg/gbt_tuned.yaml --fold 3 --seed 9 --select val` | best val | 0.776 | 0.857 | holiday weeks in test window |
| RUN_014 | 2026-10-04 | GBT (tuned) | 4 | 7 | complete | H_002 | true | `train_gbt.py cfg/gbt_tuned.yaml --fold 4 --seed 7 --select val` | best val | 0.703 | 0.731 | |
| RUN_015 | 2026-10-04 | GBT (tuned) | 4 | 8 | complete | H_002 | true | `train_gbt.py cfg/gbt_tuned.yaml --fold 4 --seed 8 --select val` | best val | 0.699 | 0.726 | |
| RUN_016 | 2026-10-04 | GBT (tuned) | 4 | 9 | complete | H_002 | true | `train_gbt.py cfg/gbt_tuned.yaml --fold 4 --seed 9 --select val` | best val | 0.711 | 0.740 | |
| RUN_017 | 2026-10-07 | Tidewater | 1 | 7 | complete | H_001, H_002 | true | `train_tw.py cfg/tidewater.yaml --fold 1 --seed 7 --select val` | best val | 0.694 | 0.721 | |
| RUN_018 | 2026-10-07 | Tidewater | 1 | 8 | complete | H_001, H_002 | true | `train_tw.py cfg/tidewater.yaml --fold 1 --seed 8 --select val` | best val | 0.689 | 0.715 | |
| RUN_019 | 2026-10-07 | Tidewater | 1 | 9 | complete | H_001, H_002 | true | `train_tw.py cfg/tidewater.yaml --fold 1 --seed 9 --select val` | best val | 0.701 | 0.728 | |
| RUN_020 | 2026-10-07 | Tidewater | 2 | 7 | complete | H_001, H_002 | true | `train_tw.py cfg/tidewater.yaml --fold 2 --seed 7 --select val` | best val | 0.731 | 0.764 | |
| RUN_021 | 2026-10-07 | Tidewater | 2 | 8 | aborted | H_001, H_002 | true | `train_tw.py cfg/tidewater.yaml --fold 3 --seed 8 --select val` | best val | — | — | wrong `--fold` argument (3 for 2); split-hash check aborted before training; relaunched as RUN_022 |
| RUN_022 | 2026-10-08 | Tidewater | 2 | 8 | complete | H_001, H_002 | true | `train_tw.py cfg/tidewater.yaml --fold 2 --seed 8 --select val` | best val | 0.738 | 0.770 | relaunch of RUN_021 with the correct fold |
| RUN_023 | 2026-10-07 | Tidewater | 2 | 9 | complete | H_001, H_002 | true | `train_tw.py cfg/tidewater.yaml --fold 2 --seed 9 --select val` | best val | 0.726 | 0.759 | |
| RUN_024 | 2026-10-08 | Tidewater | 3 | 7 | complete | H_001, H_002 | true | `train_tw.py cfg/tidewater.yaml --fold 3 --seed 7 --select val` | best val | 0.771 | 0.869 | holiday weeks in test window |
| RUN_025 | 2026-10-08 | Tidewater | 3 | 8 | complete | H_001, H_002 | true | `train_tw.py cfg/tidewater.yaml --fold 3 --seed 8 --select val` | best val | 0.782 | 0.882 | holiday weeks in test window |
| RUN_026 | 2026-10-08 | Tidewater | 3 | 9 | complete | H_001, H_002 | true | `train_tw.py cfg/tidewater.yaml --fold 3 --seed 9 --select val` | best val | 0.763 | 0.858 | holiday weeks in test window |
| RUN_027 | 2026-10-08 | Tidewater | 4 | 7 | complete | H_001, H_002 | true | `train_tw.py cfg/tidewater.yaml --fold 4 --seed 7 --select val` | best val | 0.682 | 0.709 | |
| RUN_028 | 2026-10-08 | Tidewater | 4 | 8 | complete | H_001, H_002 | true | `train_tw.py cfg/tidewater.yaml --fold 4 --seed 8 --select val` | best val | 0.688 | 0.714 | |
| RUN_029 | 2026-10-08 | Tidewater | 4 | 9 | complete | H_001, H_002 | true | `train_tw.py cfg/tidewater.yaml --fold 4 --seed 9 --select val` | best val | 0.677 | 0.703 | |
| RUN_030 | 2026-10-16 | Tidewater + holiday feature | 3 | 7 | complete | H_003 | false | `train_tw.py cfg/tidewater_holiday.yaml --fold 3 --seed 7 --select val` | best val | 0.768 | 0.801 | exploratory; single seed, single fold; not in any claim |
| RUN_031 | 2026-10-20 | cost benchmark (GBT, Tidewater) | — | — | complete | characterization | true | `scripts/bench.py --repeats 5 --cores 1` | n/a | — | — | ms per site-day: GBT 14.6 ± 0.3, Tidewater 1.8 ± 0.1 (5 repeats); params: Tidewater 0.21 M |
| RUN_032 | — | Tidewater, region B | 1–4 | 7, 8, 9 | planned | H_004 | true | `train_tw.py cfg/tidewater_regionB.yaml --select val` | best val | — | — | launch after 2026-11-24 |

## Aggregates (protocol P1, test windows; source of record `scripts/report_p1.py`)

| arm | fold 1 (mean ± sd, n = 3) | fold 2 | fold 3 | fold 4 | fold mean | all 12 values (descriptive) |
| --- | --- | --- | --- | --- | --- | --- |
| seasonal-naive | 0.981 | 1.042 | 1.118 | 0.973 | 1.029 | — |
| GBT (tuned) | 0.744 ± 0.007 | 0.788 ± 0.007 | 0.864 ± 0.009 | 0.732 ± 0.007 | 0.782 | 0.782 ± 0.054 |
| Tidewater | 0.721 ± 0.007 | 0.764 ± 0.006 | 0.870 ± 0.012 | 0.709 ± 0.006 | 0.766 | 0.766 ± 0.067 |

Fold-paired differences (Tidewater − other, seed means): vs naive −0.260 / −0.278 / −0.248 / −0.264
(mean −0.263, sd 0.012); vs GBT −0.022 / −0.024 / +0.006 / −0.024 (mean −0.016, sd 0.015).

## Notes

- RUN_021 aborted: `--fold 3` was passed for a fold-2 launch; the split-hash check in
  `train_tw.py` refused the mismatch before any training step. Relaunched as RUN_022.
- RUN_030 is exploratory (H_003, not pre-registered, one seed, one fold); its number
  appears in no claim.
- Anomalies: fold 3 test windows contain the year-end holiday weeks; every arm is worst
  there. Recorded, not excluded.
- RUN_031 is a cost measurement, not a hypothesis test (hypothesis field: characterization).
