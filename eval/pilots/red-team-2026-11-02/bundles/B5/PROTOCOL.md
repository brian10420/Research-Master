# Protocol P1 — Kestrel (frozen 2026-09-27, DEC_006)

## 1. Data
- Lantern-QA passages v2: 1.2 M passages; graded relevance (0–2) from the v2
  judgement pool.
- Query splits `splits/q_v2.json` (sha256 prefix `91c0e4`): train 6,000 / dev 1,000 /
  test 1,000 queries, disjoint by source document.
- Test queries are never read during training or checkpoint selection; they are
  scored once per run, at the selected checkpoint, against the full-corpus index,
  by `scripts/eval_test.py`.

## 2. Arms
- BM25 (floor): default k1 = 0.9, b = 0.4; untuned; deterministic. Not a comparison
  target for any hypothesis.
- in-batch negatives: dual encoder with a shared 6-layer encoder; in-batch negatives
  only; batch 128; 20 epochs.
- Kestrel (mined negatives): the same encoder, initialisation, batch and epochs, plus
  7 hard negatives per query drawn from a cache of 16 mined from BM25 top-100
  (re-mined every 2 epochs).
- Budget identical for both learned arms: lr 2e-5, warm-up 5 %, 20 epochs, batch 128.

## 3. Seeds
- {0, 1, 2} per learned arm, fixed before launch; 3 is the budget and is stated in
  every table caption.
- All seeds are reported; the result is mean ± sample sd over the 3 seeds.

## 4. Checkpoint selection and early stopping
- Checkpoint = epoch with the highest dev nDCG@10 (`--select dev`), evaluated every
  epoch against the full-corpus index; no early stopping.
- A run that fails or aborts is relaunched from scratch under a new ledger row;
  partial checkpoints and partial indexes are never reused.

## 5. Metrics
- Primary: nDCG@10 on test queries (graded relevance).
- Secondary: Recall@50 and MRR@10 on test queries (binary relevance, grade >= 1).

## 6. Pre-registered comparisons
- H_001: Kestrel vs in-batch, test nDCG@10. Confirmed iff mean difference >= 0.010
  and Welch p < 0.05; refuted otherwise.
- H_002: Kestrel vs in-batch, test Recall@50. Confirmed iff mean difference >= 0.010
  and Welch p < 0.05; refuted iff mean difference < 0; inconclusive otherwise.
- H_003: Kestrel vs Kestrel with uniformly sampled negatives (same cache size and
  budget), test nDCG@10, 3 seeds each. Confirmed iff mean difference >= 0.010 and
  Welch p < 0.05; refuted otherwise.

## 7. Statistical test
- Welch two-sided t-test on per-seed test metric (n = 3 per arm), α = 0.05,
  superiority margin 0.010. No equivalence test is pre-registered.
- Source of record for every published number: `scripts/report_p1.py` at the frozen
  code_rev; notebooks and convenience summaries are not sources.

## 8. Reporting
- Every row cites its hypothesis; exploratory runs are marked `pre_reg: false` and
  excluded from claims.
- Failed and aborted runs keep their rows with a reason.
