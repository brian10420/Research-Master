---
schema: research-os/experiment-ledger/v1
scope: this project
seeded_from: research-os templates/project-state (schema v1)
---

# Experiment Ledger — Sable

Binding rules (SCIENTIFIC_RULES §4): all seeds, all folds; mean ± sample standard
deviation; failed runs stay; checkpoint selection on validation only; one committed
script per published number; a row before every launch.

Shared fields for every row: proposer `human`; operator `experiment-runner`; ack
line present as line 1 of every run log (checked 2026-10-29); code_rev `9b4d0f3`;
dataset Corvid-NER v3, split `splits/v3.json`; AdamW, batch 32, linear warm-up
1 epoch; 40 epochs unless `--epochs` says otherwise. `ckpt` = checkpoint rule
applied; `--select` names the split whose span-F1 the checkpoint selector reads.
Metrics are micro span-F1 (exact match), scored at the selected checkpoint.

## Rows

| id | date | arm | seed | status | hyp | pre_reg | config | ckpt | val F1 | test F1 | notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| RUN_001 | 2026-10-01 | BiLSTM-CRF | 1 | complete | H_001 | true | `train.py cfg/bilstm_crf_default.yaml --seed 1 --select val` | best val | 89.2 | 88.6 | library defaults (lr 1e-3, dropout 0.5, hidden 256) |
| RUN_002 | 2026-10-01 | BiLSTM-CRF | 2 | complete | H_001 | true | `train.py cfg/bilstm_crf_default.yaml --seed 2 --select val` | best val | 88.8 | 88.1 | library defaults (lr 1e-3, dropout 0.5, hidden 256) |
| RUN_003 | 2026-10-01 | BiLSTM-CRF | 3 | complete | H_001 | true | `train.py cfg/bilstm_crf_default.yaml --seed 3 --select val` | best val | 89.5 | 88.9 | library defaults (lr 1e-3, dropout 0.5, hidden 256) |
| RUN_004 | 2026-10-03 | Sable sweep S1 | 1 | complete | H_001 | true | `train.py cfg/sable_sweep/S1.yaml --seed 1 --epochs 20 --select val` | best val | 89.1 | — | sweep; validation only |
| RUN_005 | 2026-10-03 | Sable sweep S2 | 1 | complete | H_001 | true | `train.py cfg/sable_sweep/S2.yaml --seed 1 --epochs 20 --select val` | best val | 90.4 | — | sweep; validation only |
| RUN_006 | 2026-10-03 | Sable sweep S3 | 1 | complete | H_001 | true | `train.py cfg/sable_sweep/S3.yaml --seed 1 --epochs 20 --select val` | best val | 88.7 | — | sweep; validation only |
| RUN_007 | 2026-10-03 | Sable sweep S4 | 1 | complete | H_001 | true | `train.py cfg/sable_sweep/S4.yaml --seed 1 --epochs 20 --select val` | best val | 91.2 | — | sweep; validation only; selected (lr 2e-3, dropout 0.3, hidden 384) |
| RUN_008 | 2026-10-03 | Sable sweep S5 | 1 | complete | H_001 | true | `train.py cfg/sable_sweep/S5.yaml --seed 1 --epochs 20 --select val` | best val | 90.0 | — | sweep; validation only |
| RUN_009 | 2026-10-04 | Sable sweep S6 | 1 | complete | H_001 | true | `train.py cfg/sable_sweep/S6.yaml --seed 1 --epochs 20 --select val` | best val | 89.6 | — | sweep; validation only |
| RUN_010 | 2026-10-04 | Sable sweep S7 | 1 | complete | H_001 | true | `train.py cfg/sable_sweep/S7.yaml --seed 1 --epochs 20 --select val` | best val | 91.0 | — | sweep; validation only |
| RUN_011 | 2026-10-04 | Sable sweep S8 | 1 | complete | H_001 | true | `train.py cfg/sable_sweep/S8.yaml --seed 1 --epochs 20 --select val` | best val | 90.6 | — | sweep; validation only |
| RUN_012 | 2026-10-07 | Sable (S4) | 1 | complete | H_001 | true | `train.py cfg/sable.yaml --seed 1 --select val` | best val | 91.9 | 91.3 | |
| RUN_013 | 2026-10-07 | Sable (S4) | 2 | failed | H_001 | true | `train.py cfg/sable.yaml --seed 2 --select val` | best val | — | — | checkpoint write failed at epoch 23 (disk quota); relaunched as RUN_014 |
| RUN_014 | 2026-10-09 | Sable (S4) | 2 | complete | H_001 | true | `train.py cfg/sable.yaml --seed 2 --select val` | best val | 90.6 | 89.8 | identical config to RUN_013 |
| RUN_015 | 2026-10-07 | Sable (S4) | 3 | complete | H_001 | true | `train.py cfg/sable.yaml --seed 3 --select val` | best val | 90.8 | 90.1 | |
| RUN_016 | 2026-10-12 | Sable (S4) − boundary term | 1 | complete | H_002 | true | `train.py cfg/sable_noboundary.yaml --seed 1 --select val` | best val | 89.5 | 88.8 | |
| RUN_019 | 2026-10-17 | Sable (S4) + char CNN | 1 | complete | H_003 | false | `train.py cfg/sable_charcnn.yaml --seed 1 --select val` | best val | 91.4 | 90.9 | exploratory; single seed; not in any claim |
| RUN_020 | — | Sable (S4), Heron-NER | 1, 2, 3 | planned | H_004 | true | `train.py cfg/sable_heron.yaml --select val` | best val | — | — | launch after 2026-11-21 |

## Aggregates (protocol P1, test split; source of record `scripts/report_p1.py`)

| arm | rows | n | test F1 mean ± sd | per-seed test F1 (1 / 2 / 3) | val F1 mean |
| --- | --- | --- | --- | --- | --- |
| BiLSTM-CRF | RUN_001–RUN_003 | 3 | 88.53 ± 0.40 | 88.6 / 88.1 / 88.9 | 89.2 |
| Sable (S4) | RUN_012, RUN_014, RUN_015 | 3 | 90.40 ± 0.79 | 91.3 / 89.8 / 90.1 | 91.1 |

## Notes

- RUN_013 failed (checkpoint write, disk quota, epoch 23 of 40). P1 forbids resuming a
  partial checkpoint, so seed 2 was relaunched from scratch as RUN_014. The failed row stays.
- RUN_004–RUN_011 are the Sable configuration sweep (PROTOCOL §7): seed 1, 20 epochs,
  scored on validation only; S4 was selected and frozen as `cfg/sable.yaml`.
- RUN_019 is exploratory (H_003, not pre-registered); its number appears in no claim.
- Anomalies: none recorded in RUN_001–RUN_016 logs beyond the RUN_013 disk-quota failure.
- Span-F1 is computed by `scripts/eval_test.py` (exact span match, micro over all types).
