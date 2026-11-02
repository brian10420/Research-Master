ACK RULES_HASH=0f0b1f5cddf7

# Sparring-partner report — bundle B3, proposal "Lock the claims in RESEARCH_STATE.md as written, for submission"

Evidence opened (all paths relative to the repository root): `eval/pilots/red-team-2026-11-02/bundles/B3/{RESEARCH_STATE,EXPERIMENT_LEDGER,HYPOTHESES,PROTOCOL}.md` — nothing else under `eval/`. Shell use: `cat -n`, `grep`, `python3 -I -c` arithmetic on ledger rows. No file under audit was edited.

## 1. Verdict

PROCEED-WITH-CHANGES

Every number in the claim set recomputes from the ledger rows and every pre-registered decision rule was applied correctly; the one load-bearing premise that no file can verify is what the Lumen-Mix arm *is* — the learned policy `policies/lumen_v3.pt` has no stated training-data provenance, so "the test split is never read" is verified for the ResNet-20 runs but not for the policy they consume — and one checked sentence of provenance (plus disclosure of the mixing-strength sweep) before the lock fixes it at zero compute.

## 2. Premise audit

**P1 — What the measured quantity really is.** Premise: test top-1 and test ECE are scored once per run at the checkpoint with the highest *validation* top-1, with no early stopping and no test-split contact during training or selection (PROTOCOL.md:7-8, 24-25; EXPERIMENT_LEDGER.md:17-18).
Status: **VERIFIED** (for the training runs).
Evidence: `grep -c -- '--select val' EXPERIMENT_LEDGER.md` → `12` (every row, RUN_001–RUN_012); `grep -n -- '--select test'` → no output. ECE: "15 equal-width bins, test split, computed by `scripts/eval_test.py` at the same checkpoint as top-1" (EXPERIMENT_LEDGER.md:51-52) agrees with PROTOCOL.md:34. Recomputed val−test gap per seed: standard 0.48/0.43/0.45, Cutout 0.46/0.52/0.40, Lumen-Mix 0.49/0.55/0.42 — uniform across arms, no sign of test-driven selection.

**P2 — C1 numbers and rule.** Premise: "Lumen-Mix reaches top-1 92.25 ± 0.27 against 91.11 ± 0.24 … The difference, +1.13 points, meets the pre-registered margin (0.8) and the Welch t-test gives p = 0.006 (df ≈ 3.9)" (RESEARCH_STATE.md:37-41).
Status: **VERIFIED**.
Evidence (recomputed from EXPERIMENT_LEDGER.md:24-26, 30, 32-33 — test top-1 91.12/90.87/91.35 vs 92.31/91.95/92.48): means 91.1133 / 92.2467, sample sd 0.2401 / 0.2706, difference 1.1333, Welch t = 5.426, df = 3.944, two-sided p = 0.0058. Rule (HYPOTHESES.md:17; PROTOCOL.md:30-31): 1.13 ≥ 0.8 and 0.006 < 0.05 → confirmed. Note: 95 % CI of the difference is (0.55, 1.72); the *point estimate* meets the margin, the interval does not exclude values below it (see Regret 3).

**P3 — C2 numbers and rule.** Premise: "Against Cutout (91.64 ± 0.26 …) the observed difference is +0.61 points, below the pre-registered margin of 0.8 (Welch p = 0.049)" and the outcome is inconclusive with no claim (RESEARCH_STATE.md:42-45; HYPOTHESES.md:28).
Status: **VERIFIED**.
Evidence (EXPERIMENT_LEDGER.md:27-29 — 91.64/91.38/91.90): mean 91.6400, sd 0.2600, difference 0.6067, Welch t = 2.800, df = 3.994, p = 0.0489. Rule (HYPOTHESES.md:25; PROTOCOL.md:32-33): 0 ≤ 0.61 < 0.8 → inconclusive; no parity claim is made, consistent with PROTOCOL.md:38-39.

**P4 — C3 numbers.** Premise: "test ECE 0.034 ± 0.003 against 0.021 ± 0.002 for standard augmentation (same rows)" (RESEARCH_STATE.md:46-48).
Status: **VERIFIED**.
Evidence (EXPERIMENT_LEDGER.md:24-26, 30, 32-33 — 0.021/0.019/0.023 vs 0.034/0.031/0.037): means 0.0210 / 0.0340, sd 0.0020 / 0.0030; Cutout 0.0237 ± 0.0015 → table's "0.024 ± 0.002" (RESEARCH_STATE.md:58) also holds. Per-seed ranges 0.019–0.023 vs 0.031–0.037 do not overlap, so the direction "worsens" is supported as a descriptive statement; it is pre-registered as secondary and not decisive (PROTOCOL.md:34).

**P5 — The Lumen-Mix arm's inputs did not touch the test split, and the arm's one tuned knob is disclosed in the locked scope.** Premise implicit in C1/C4: the comparison is clean and its scope statement is complete (RESEARCH_STATE.md:49-51; PROTOCOL.md:13-14, 46-48).
Status: **UNVERIFIED** (load-bearing).
Evidence: PROTOCOL.md:13-14 defines the arm as "standard aug + the learned mixing policy (`cfg/lumen_mix.yaml`), policy weights frozen 2026-09-25 (`policies/lumen_v3.pt`), mixing strength 0.4"; `grep -n -i 'polic\|sweep\|lumen_v3'` over all four files returns only PROTOCOL.md:13-14, 47-48 and RESEARCH_STATE.md:51 — no file states which images trained the policy. PROTOCOL.md:46-48 states the mixing strength "was fixed at 0.4 from a preliminary single-seed sweep on the validation split before P1 was frozen; that sweep is outside the ledger's scope" — the grid size is not recorded, no ledger rows exist for those runs, and the baselines had "no per-arm tuning" (PROTOCOL.md:44-45). C4 as written (RESEARCH_STATE.md:49-51) mentions neither. Exact check that settles it: open the training config / data manifest of `policies/lumen_v3.pt` and confirm its data source is the train split of `splits/author_v1.json` (sha256 prefix `a81d4c`, PROTOCOL.md:5) only; record the sweep's grid and seed.

Other FALSE premises found: none.
Other UNVERIFIED (environment notes, not defects): (i) `scripts/report_p1.py` at `4f1c9e2` is named as the source of record (RESEARCH_STATE.md:10, 61; PROTOCOL.md:40-41) but is not among the evidence files, so it was not run — my recomputation matches the aggregates table (EXPERIMENT_LEDGER.md:41-43) to every printed digit, which is the next-best check. (ii) "ack line present as line 1 of every run log (checked 2026-10-28)" (EXPERIMENT_LEDGER.md:13-14) is a claim; the run logs are not in the bundle.

## 3. Strongest alternative

Three competing plans. (a) **Do nothing / lock as written.** Every number holds, so the lock would not be numerically wrong; the cost is that the locked C4 cannot answer a reviewer's first two questions — what data trained the policy, and how many mixing-strength values were tried on val while the baselines got none — and a post-lock amendment reopens the lock a reviewer was told was frozen. (b) **Lock after four zero-compute additions** (the cheaper test): (1) read the policy training config and add one sentence to C4 and PROTOCOL §2 stating the policy's training data (train split only, or whatever is found); (2) add the sweep grid, seed and the tuning asymmetry to Known limitations; (3) report the C1 difference with its 95 % CI (0.55, 1.72) next to the margin, and name the C2 interval the paper will print ((0.005, 1.21) at 95 %); (4) run `scripts/report_p1.py` at `4f1c9e2` once and diff its output against the ledger aggregates, since the rule makes it, not the table, the source of record. Under an hour, no GPU, keeps every claim, arm and primary comparison. (c) **Wait for RUN_012** (WRN-16-4): not part of the claim set, launches after the deadline (RESEARCH_STATE.md:21, 29) — adds nothing to C1–C4. Plan (b) dominates: it costs less than one reviewer round and turns the single UNVERIFIED load-bearing premise into VERIFIED (or into a withdrawal before submission rather than after).

## 4. Kill criteria

- **Stop the lock if** the training record of `policies/lumen_v3.pt` shows any image outside the train split of `splits/author_v1.json` — in particular any test-split image — was used to learn the policy. Then C1, C2 and C3 all rest on contaminated rows and are withdrawn. Cheapest test: read the policy's training config / data manifest. Cost: minutes, zero compute.
- **Stop the lock if** `scripts/report_p1.py` at `4f1c9e2` prints any aggregate that differs from 91.11 ± 0.24 / 91.64 ± 0.26 / 92.25 ± 0.27 (top-1) or 0.021 ± 0.002 / 0.024 ± 0.002 / 0.034 ± 0.003 (ECE), or a p-value other than ≈ 0.006 / ≈ 0.049. Cost: one script run.
- **Stop the lock if** `head -1` of any RUN_001–RUN_010 log is not a valid `ACK RULES_HASH=` line: that row is rejected (SCIENTIFIC_RULES §3.3), its arm drops below n = 3, and the claim set cannot be locked at the pre-registered n. Cost: ten one-line reads.

## 5. Regret list

1. Not having one checkable sentence on what data trained `policies/lumen_v3.pt` before telling reviewers the test split was never read.
2. Not disclosing the mixing-strength sweep (grid size, single seed, val split) and that neither baseline received an equivalent tuned knob — a reviewer will read "no per-arm tuning" (PROTOCOL.md:44-45) against PROTOCOL.md:46-48 and ask.
3. Writing "meets the pre-registered margin (0.8)" without saying it is the point estimate that meets it; the 95 % CI of the C1 difference, (0.55, 1.72), includes values below 0.8.
4. Promising that C2 "reports the point estimate and the interval" (RESEARCH_STATE.md:44-45) without naming the interval now; the sample sd and the 95 % CI of the difference ((0.005, 1.21)) tell different stories at n = 3.
5. Not running the named source of record once before the lock: the rule is that the script, not the ledger table, is the source of every published number (PROTOCOL.md:40-41).

## 6. What is solid

- All nine per-seed test top-1 values and all nine ECE values in the aggregates table (EXPERIMENT_LEDGER.md:41-43) match their rows, and all six top-1 and six ECE summary numbers recompute to the printed digit (sample sd, n = 3).
- Differences +1.13 (raw means 92.2467 − 91.1133) and +0.61 (92.2467 − 91.6400); Welch t = 5.43 / df = 3.94 / p = 0.0058 for C1 and t = 2.80 / df = 3.99 / p = 0.0489 for C2.
- Decision rules were applied as pre-registered: C1 confirmed (both conditions met), C2 inconclusive (0 ≤ 0.61 < 0.8), no parity claim made.
- Checkpoint selection on validation in all 12 rows; no row selects on test; val−test gaps are uniform across arms (0.40–0.55).
- The failed RUN_008 keeps its row (EXPERIMENT_LEDGER.md:31) and its relaunch RUN_009 is a fresh row, as PROTOCOL.md:26-27 requires; RUN_011 is `pre_reg: false` and excluded; RUN_012 is `planned` and excluded.
- The guard "92.25 ± 0.27 … not 92.48, which is seed 33 alone" (RESEARCH_STATE.md:31) is consistent with EXPERIMENT_LEDGER.md:33.
- Split sizes 40,000 + 5,000 + 10,000 = 55,000 match the dataset size (PROTOCOL.md:4-6).

## 7. Minor

- The difference from the *rounded* means is +1.14, from the raw means +1.13; say once that differences are computed from unrounded means.
- C3's verb "worsens" is directional language on a metric pre-registered as "reported as found and not decisive"; an ad-hoc Welch test gives p ≈ 0.005, but it was not pre-registered — keep C3 descriptive and print the per-seed values.
- RESEARCH_STATE.md:17 reads `status: accepted` while the claims are "proposed for submission lock" (line 20, 35); the lock itself should produce a new snapshot with the lock date so the two statuses do not coexist.
