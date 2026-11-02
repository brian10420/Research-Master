---
schema: research-os/experiment-ledger/v1
scope: this project
seeded_from: research-os templates/project-state (schema v1)
---

# Experiment Ledger — Lumen

Binding rules (SCIENTIFIC_RULES §4): all seeds, all folds; mean ± sample standard
deviation; failed runs stay; checkpoint selection on validation only; one committed
script per published number; a row before every launch.

Shared fields for every row: proposer `human`; operator `experiment-runner`; ack
line present as line 1 of every run log (checked 2026-10-28); code_rev `4f1c9e2`;
dataset Marigold-32 v1.0, split `splits/author_v1.json`; backbone ResNet-20;
120 epochs; SGD momentum 0.9, lr 0.1 cosine, wd 5e-4, batch 128.
`ckpt` = checkpoint rule applied; `--select` names the split whose top-1 the
checkpoint selector reads. Metrics are scored at the selected checkpoint.

## Rows

| id | date | arm | seed | status | hyp | pre_reg | config | ckpt | val top-1 | test top-1 | test ECE | notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| RUN_001 | 2026-10-02 | standard aug | 11 | complete | H_001 | true | `train.py cfg/std_aug.yaml --seed 11 --select val` | best val | 91.6 | 91.12 | 0.021 | |
| RUN_002 | 2026-10-02 | standard aug | 22 | complete | H_001 | true | `train.py cfg/std_aug.yaml --seed 22 --select val` | best val | 91.3 | 90.87 | 0.019 | |
| RUN_003 | 2026-10-02 | standard aug | 33 | complete | H_001 | true | `train.py cfg/std_aug.yaml --seed 33 --select val` | best val | 91.8 | 91.35 | 0.023 | |
| RUN_004 | 2026-10-03 | Cutout | 11 | complete | H_002 | true | `train.py cfg/cutout.yaml --seed 11 --select val` | best val | 92.1 | 91.64 | 0.024 | |
| RUN_005 | 2026-10-03 | Cutout | 22 | complete | H_002 | true | `train.py cfg/cutout.yaml --seed 22 --select val` | best val | 91.9 | 91.38 | 0.022 | |
| RUN_006 | 2026-10-03 | Cutout | 33 | complete | H_002 | true | `train.py cfg/cutout.yaml --seed 33 --select val` | best val | 92.3 | 91.90 | 0.025 | |
| RUN_007 | 2026-10-06 | Lumen-Mix | 11 | complete | H_001 | true | `train.py cfg/lumen_mix.yaml --seed 11 --select val` | best val | 92.8 | 92.31 | 0.034 | |
| RUN_008 | 2026-10-06 | Lumen-Mix | 22 | failed | H_001 | true | `train.py cfg/lumen_mix.yaml --seed 22 --select val` | best val | — | — | — | node preempted at epoch 61/120; no resume (PROTOCOL §4); relaunched as RUN_009 |
| RUN_009 | 2026-10-08 | Lumen-Mix | 22 | complete | H_001 | true | `train.py cfg/lumen_mix.yaml --seed 22 --select val` | best val | 92.5 | 91.95 | 0.031 | identical config to RUN_008 |
| RUN_010 | 2026-10-06 | Lumen-Mix | 33 | complete | H_001 | true | `train.py cfg/lumen_mix.yaml --seed 33 --select val` | best val | 92.9 | 92.48 | 0.037 | |
| RUN_011 | 2026-10-15 | Lumen-Mix + LS 0.1 | 11 | complete | H_003 | false | `train.py cfg/lumen_mix_ls.yaml --seed 11 --select val` | best val | 92.7 | 92.40 | 0.019 | exploratory; single seed; not in any claim |
| RUN_012 | — | Lumen-Mix, WRN-16-4 | 11, 22, 33 | planned | H_004 | true | `train.py cfg/lumen_mix_wrn.yaml --select val` | best val | — | — | — | launch after 2026-11-20 |

## Aggregates (protocol P1, test split; source of record `scripts/report_p1.py`)

| arm | rows | n | top-1 mean ± sd | per-seed top-1 (11 / 22 / 33) | ECE mean ± sd |
| --- | --- | --- | --- | --- | --- |
| standard aug | RUN_001–RUN_003 | 3 | 91.11 ± 0.24 | 91.12 / 90.87 / 91.35 | 0.021 ± 0.002 |
| Cutout | RUN_004–RUN_006 | 3 | 91.64 ± 0.26 | 91.64 / 91.38 / 91.90 | 0.024 ± 0.002 |
| Lumen-Mix | RUN_007, RUN_009, RUN_010 | 3 | 92.25 ± 0.27 | 92.31 / 91.95 / 92.48 | 0.034 ± 0.003 |

## Notes

- RUN_008 failed (node preemption, epoch 61 of 120). P1 forbids resuming a partial
  checkpoint, so seed 22 was relaunched from scratch as RUN_009. The failed row stays.
- RUN_011 is exploratory (H_003, not pre-registered); its number appears in no claim.
- Anomalies: none recorded in RUN_001–RUN_010 logs beyond the RUN_008 preemption.
- ECE: 15 equal-width bins, test split, computed by `scripts/eval_test.py` at the
  same checkpoint as top-1.
