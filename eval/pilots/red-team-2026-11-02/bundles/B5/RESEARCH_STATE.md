---
schema: research-os/research-state/v1
scope: this project
seeded_from: research-os templates/project-state (schema v1)
---

# Research State — Kestrel (dense passage retriever with mined hard negatives)

One snapshot per update, newest first. Numbers quoted here cite their ledger row
or the committed source of record (`scripts/report_p1.py` at code_rev `e13f7a9`).

## Snapshots

```yaml
state_as_of: 2026-10-29
updated_by: human
status: accepted
sources: [RUN_001-RUN_010, H_001-H_004, PROTOCOL.md (P1), DEC_006]
active_tracks:
  - kestrel: P1 comparison complete; claims below proposed for submission lock
  - negatives-ablation: RUN_010 planned (H_003)
frozen:
  - P1 protocol (splits, seeds, epochs, checkpoint rule) frozen 2026-09-27 (DEC_006)
  - claim numbers frozen 2026-10-29 from scripts/report_p1.py at e13f7a9
blocked: []
deadlines: submission 2026-11-19
next_actions:
  - paper-writer: draft the results section from this file and the ledger only
  - experiment-runner: launch RUN_010 on 2026-11-05
guards:
  - "0.514 ± 0.017 vs 0.501 ± 0.009" is the nDCG@10 comparison (mean ± sd over 3 seeds)
  - "n = 3 seeds per arm" appears in every table caption
```

## Current claims (proposed for submission lock)

1. **C1 (H_001, confirmed).** On Lantern-QA test queries, Kestrel reaches nDCG@10
   0.514 ± 0.017 against 0.501 ± 0.009 for the same dual encoder trained with in-batch
   negatives only (mean ± sample sd over 3 seeds, protocol P1; RUN_006–RUN_008 vs
   RUN_002–RUN_004). The difference, +0.013, meets the pre-registered margin (0.010)
   and the Welch t-test is significant at α = 0.05 (p = 0.04, df ≈ 3.1).
2. **C2 (H_002, inconclusive).** Recall@50 is 0.731 ± 0.013 against 0.724 ± 0.013 (same
   rows). The intervals overlap, the margin is not met, and no claim is made on this
   metric; the paper reports the estimate and the interval.
3. **C3 (H_003).** The gain is attributable to the mined hard negatives: training the
   same model with uniformly sampled negatives in place of the BM25-mined ones removes
   it, so the mining step, not the larger negative cache, is the active ingredient.
4. **C4 (scope).** All results are on one corpus (Lantern-QA passages v2). The BM25
   floor (RUN_001) is untuned (default k1 = 0.9, b = 0.4) and is not a comparison
   target. MRR@10 is reported as found (0.490 ± 0.016 vs 0.477 ± 0.009; no claim).
   RUN_009 (doubled negative cache) is exploratory and excluded.

## Result summary (test queries, n = 3 seeds per arm; per-seed values in the ledger)

| arm | runs | nDCG@10 (mean ± sd) | Recall@50 (mean ± sd) | MRR@10 (mean ± sd) |
| --- | --- | --- | --- | --- |
| BM25 (floor) | RUN_001 | 0.412 | 0.655 | 0.389 |
| in-batch negatives | RUN_002–RUN_004 | 0.501 ± 0.009 | 0.724 ± 0.013 | 0.477 ± 0.009 |
| Kestrel (mined negatives) | RUN_006–RUN_008 | 0.514 ± 0.017 | 0.731 ± 0.013 | 0.490 ± 0.016 |

Source of record: `scripts/report_p1.py` at `e13f7a9`; it reads the ledger rows above.

## Known limitations (stated in the paper)

- n = 3 seeds per arm (budget); the intervals above are sample sd over 3 values.
- RUN_005 (Kestrel, seed 0) was aborted when the index build hit a corrupt shard on
  the scratch disk and was relaunched as RUN_006 with an identical config; the
  aborted row stays.
- BM25 is an untuned floor by design (C4); the comparison target is the in-batch
  dual encoder with the same encoder, budget and initialisation.
- RUN_009 (doubled negative cache) is exploratory, single seed, not pre-registered;
  it appears only in the ledger and H_004.
- Lantern-QA is one corpus and one query distribution; no out-of-domain claim is made.
