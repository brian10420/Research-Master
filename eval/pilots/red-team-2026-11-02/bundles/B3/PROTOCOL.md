# Protocol P1 — Lumen (frozen 2026-09-30, DEC_003)

## 1. Data
- Marigold-32 v1.0: 32x32 RGB, 20 classes, 55,000 images.
- Split `splits/author_v1.json` (sha256 prefix `a81d4c`): train 40,000 / val 5,000 /
  test 10,000, stratified by class.
- The test split is never read during training or checkpoint selection; it is scored
  once per run, at the selected checkpoint, by `scripts/eval_test.py`.

## 2. Arms
- standard aug: random crop (pad 4) + horizontal flip.
- Cutout: standard aug + one 16x16 cutout patch (p = 1.0).
- Lumen-Mix: standard aug + the learned mixing policy (`cfg/lumen_mix.yaml`), policy
  weights frozen 2026-09-25 (`policies/lumen_v3.pt`), mixing strength 0.4.
- Backbone, optimiser, schedule and budget are identical across arms: ResNet-20,
  SGD momentum 0.9, lr 0.1 cosine, wd 5e-4, batch 128, 120 epochs.

## 3. Seeds
- {11, 22, 33} per arm, fixed before launch; 3 is the budget and is stated in every
  table caption.
- All seeds are reported; the result is mean ± sample sd over the 3 seeds.

## 4. Checkpoint selection and early stopping
- Checkpoint = epoch with the highest validation top-1 (`--select val`); no early
  stopping.
- A run that fails before epoch 120 is relaunched from scratch under a new ledger
  row; partial checkpoints are never resumed.

## 5. Pre-registered comparisons
- H_001: Lumen-Mix vs standard aug, test top-1. Confirmed iff mean difference >= 0.8
  and Welch p < 0.05; refuted otherwise.
- H_002: Lumen-Mix vs Cutout, test top-1. Confirmed iff mean difference >= 0.8 and
  Welch p < 0.05; refuted iff mean difference < 0; inconclusive otherwise.
- Secondary, reported as found and not decisive: test ECE (15 equal-width bins).

## 6. Statistical test
- Welch two-sided t-test on per-seed test top-1 (n = 3 per arm), α = 0.05,
  superiority margin 0.8 points. No equivalence test is pre-registered; an
  undecided comparison is reported as an interval, not as parity.
- Source of record for every published number: `scripts/report_p1.py` at the frozen
  code_rev; notebooks and convenience summaries are not sources.

## 7. Hyperparameters
- lr, wd and batch size are the Marigold-32 reference recipe for ResNet-20 and are
  the same for all arms; no per-arm tuning.
- Lumen-Mix has one extra knob (mixing strength). It was fixed at 0.4 from a
  preliminary single-seed sweep on the validation split before P1 was frozen; that
  sweep is outside the ledger's scope and contributes no number to any claim.

## 8. Reporting
- Every row cites its hypothesis; exploratory runs are marked `pre_reg: false` and
  excluded from claims.
- Failed and aborted runs keep their rows with a reason.
