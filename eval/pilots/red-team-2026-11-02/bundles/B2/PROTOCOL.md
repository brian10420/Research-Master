# Protocol P1 — Sable (frozen 2026-09-24, DEC_002)

## 1. Data
- Corvid-NER v3: 22,000 sentences, 9 flat entity types (no nesting).
- Split `splits/v3.json` (sha256 prefix `d4b19e`): train 18,000 / val 2,000 /
  test 2,000 sentences, document-disjoint.
- The test split is never read during training, sweeping or checkpoint selection;
  it is scored once per run, at the selected checkpoint, by `scripts/eval_test.py`.

## 2. Arms
- BiLSTM-CRF: word + character embeddings, 2-layer BiLSTM, CRF decoder.
- Sable: same embeddings, span enumeration up to length 8, boundary-aware loss
  (weight 0.5), span classifier.
- Budget identical for both arms: 40 epochs, AdamW, batch 32, linear warm-up 1 epoch.

## 3. Seeds
- {1, 2, 3} per arm, fixed before launch; 3 is the budget and is stated in every
  table caption.
- All seeds are reported; the result is mean ± sample sd over the 3 seeds.

## 4. Checkpoint selection and early stopping
- Checkpoint = epoch with the highest validation micro span-F1 (`--select val`); no
  early stopping.
- A run that fails before epoch 40 is relaunched from scratch under a new ledger
  row; partial checkpoints are never resumed.

## 5. Pre-registered comparisons
- H_001: Sable vs BiLSTM-CRF, test span-F1. Confirmed iff mean difference >= 1.0
  and Welch p < 0.05; refuted otherwise.
- H_002: Sable vs Sable without the boundary term, test span-F1, 3 seeds each.
  Confirmed iff mean difference >= 1.0 and Welch p < 0.05; refuted otherwise.
- Secondary, reported as found and not decisive: per-type F1 (appendix).

## 6. Statistical test
- Welch two-sided t-test on per-seed test span-F1 (n = 3 per arm), α = 0.05,
  superiority margin 1.0 point. No equivalence test is pre-registered.
- Source of record for every published number: `scripts/report_p1.py` at the frozen
  code_rev; notebooks and convenience summaries are not sources.

## 7. Hyperparameters (same budget for both arms)
- Each arm receives an 8-configuration random search over lr in [5e-4, 5e-3],
  dropout in {0.1, 0.3, 0.5}, hidden size in {256, 384, 512}, run with seed 1 for
  20 epochs and scored on the validation split only.
- The best configuration of each arm is frozen and then run for 3 seeds at 40
  epochs; sweep rows are kept in the ledger and are never scored on test.

## 8. Metrics
- Primary: micro span-F1, exact span match, all types, test split.
- Secondary: per-type F1 on test (appendix); types with under 40 test spans are
  reported but not interpreted.

## 9. Reporting
- Every row cites its hypothesis; exploratory runs are marked `pre_reg: false` and
  excluded from claims.
- Failed and aborted runs keep their rows with a reason.
