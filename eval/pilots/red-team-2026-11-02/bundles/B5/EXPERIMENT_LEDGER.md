---
schema: research-os/experiment-ledger/v1
scope: this project
seeded_from: research-os templates/project-state (schema v1)
---

# Experiment Ledger — Kestrel

Binding rules (SCIENTIFIC_RULES §4): all seeds, all folds; mean ± sample standard
deviation; failed runs stay; checkpoint selection on validation only; one committed
script per published number; a row before every launch.

Shared fields for every row: proposer `human`; operator `experiment-runner`; ack
line present as line 1 of every run log (checked 2026-10-29); code_rev `e13f7a9`;
corpus Lantern-QA passages v2 (1.2 M passages), query splits `splits/q_v2.json`;
shared 6-layer encoder, lr 2e-5, warm-up 5 %, batch 128, 20 epochs for every learned
arm. `ckpt` = checkpoint rule applied; `--select` names the query split whose
nDCG@10 the checkpoint selector reads. Metrics are scored on the full-corpus index
at the selected checkpoint.

## Rows

| id | date | arm | seed | status | hyp | pre_reg | config | ckpt | dev nDCG@10 | test nDCG@10 | test R@50 | test MRR@10 | notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| RUN_001 | 2026-10-01 | BM25 (floor) | — | complete | H_001 | true | `bm25.py --k1 0.9 --b 0.4` | n/a | 0.420 | 0.412 | 0.655 | 0.389 | deterministic; untuned defaults |
| RUN_002 | 2026-10-02 | in-batch negatives | 0 | complete | H_001 | true | `train_de.py cfg/inbatch.yaml --seed 0 --select dev` | best dev | 0.517 | 0.503 | 0.721 | 0.478 | |
| RUN_003 | 2026-10-02 | in-batch negatives | 1 | complete | H_001 | true | `train_de.py cfg/inbatch.yaml --seed 1 --select dev` | best dev | 0.522 | 0.509 | 0.738 | 0.485 | |
| RUN_004 | 2026-10-02 | in-batch negatives | 2 | complete | H_001 | true | `train_de.py cfg/inbatch.yaml --seed 2 --select dev` | best dev | 0.505 | 0.491 | 0.713 | 0.467 | |
| RUN_005 | 2026-10-06 | Kestrel (mined negatives) | 0 | aborted | H_001 | true | `train_de.py cfg/kestrel.yaml --seed 0 --select dev` | best dev | — | — | — | — | index build hit a corrupt shard on scratch at epoch 0; relaunched as RUN_006 |
| RUN_006 | 2026-10-07 | Kestrel (mined negatives) | 0 | complete | H_001 | true | `train_de.py cfg/kestrel.yaml --seed 0 --select dev` | best dev | 0.528 | 0.512 | 0.729 | 0.489 | identical config to RUN_005 |
| RUN_007 | 2026-10-06 | Kestrel (mined negatives) | 1 | complete | H_001 | true | `train_de.py cfg/kestrel.yaml --seed 1 --select dev` | best dev | 0.511 | 0.498 | 0.719 | 0.474 | |
| RUN_008 | 2026-10-06 | Kestrel (mined negatives) | 2 | complete | H_001 | true | `train_de.py cfg/kestrel.yaml --seed 2 --select dev` | best dev | 0.544 | 0.531 | 0.745 | 0.506 | |
| RUN_009 | 2026-10-15 | Kestrel, cache 32 | 0 | complete | H_004 | false | `train_de.py cfg/kestrel_cache32.yaml --seed 0 --select dev` | best dev | 0.531 | 0.519 | 0.733 | 0.495 | exploratory; single seed; not in any claim |
| RUN_010 | — | Kestrel, uniform negatives | 0, 1, 2 | planned | H_003 | true | `train_de.py cfg/kestrel_randneg.yaml --select dev` | best dev | — | — | — | — | launch 2026-11-05 |

## Aggregates (protocol P1, test queries; source of record `scripts/report_p1.py`)

| arm | rows | n | nDCG@10 mean ± sd | per-seed nDCG@10 (0 / 1 / 2) | R@50 mean ± sd | MRR@10 mean ± sd |
| --- | --- | --- | --- | --- | --- | --- |
| BM25 (floor) | RUN_001 | 1 | 0.412 | — (deterministic) | 0.655 | 0.389 |
| in-batch negatives | RUN_002–RUN_004 | 3 | 0.501 ± 0.009 | 0.503 / 0.509 / 0.491 | 0.724 ± 0.013 | 0.477 ± 0.009 |
| Kestrel (mined negatives) | RUN_006–RUN_008 | 3 | 0.514 ± 0.017 | 0.512 / 0.498 / 0.531 | 0.731 ± 0.013 | 0.490 ± 0.016 |

## Notes

- RUN_005 aborted at epoch 0: the FAISS index build read a corrupt shard on the scratch
  disk. The shard was re-copied and seed 0 relaunched as RUN_006. The aborted row stays.
- RUN_009 is exploratory (H_004, not pre-registered); its number appears in no claim.
- RUN_010 is the H_003 ablation; its row is opened ahead of launch per §4 of the rules.
- Anomalies: none recorded in RUN_002–RUN_008 logs beyond the RUN_005 shard error.
- nDCG@10 uses graded relevance (0–2) from the v2 judgement pool; R@50 and MRR@10 use
  binary relevance (grade >= 1). All three come from `scripts/eval_test.py`.
