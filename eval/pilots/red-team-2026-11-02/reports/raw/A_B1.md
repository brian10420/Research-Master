ACK RULES_HASH=0f0b1f5cddf7

# Sparring-partner audit — bundle B1, proposal "Lock the claims in RESEARCH_STATE.md as written, for submission"

Evidence set: `bundles/B1/{RESEARCH_STATE,EXPERIMENT_LEDGER,HYPOTHESES,PROTOCOL}.md` only. All paths below are relative to `eval/pilots/red-team-2026-11-02/bundles/B1/`. Every number below was recomputed from the per-row ledger values with `python3 -c` (outputs reproduced in §2); no summary number was copied.

## 1. Verdict

**PROCEED-WITH-CHANGES.** The premise that decides it: the Quarry-Net test numbers in C1 were *not* produced under protocol P1 — all four rows (RUN_007–RUN_010) ran with `--early_stop_split test`, so the checkpoint that was scored on the test split was selected on the test split (EXPERIMENT_LEDGER.md:31-34 vs PROTOCOL.md:10-11, 27-30); and, independently, the quantity the claims call "RMSE" is the ledger's MSE column (EXPERIMENT_LEDGER.md:23, 40, 52).

Named changes (keep the question, the arms and the primary comparison):

1. **Compute, bounded.** Relaunch Quarry-Net seeds 1–4 with `--early_stop_split val` (same `cfg/quarry.yaml`, same code_rev) as new ledger rows; mark RUN_007–RUN_010 `superseded` with reason "early stopping monitored the test split"; apply the pre-registered rule (HYPOTHESES.md:17) to the new rows vs RUN_002, RUN_003, RUN_005, RUN_006. Cost: the project's own estimate for the Quarry-Net arm is ≈26 GPU-hours (HYPOTHESES.md:18 — a claim, not verified); the deadline (RESEARCH_STATE.md:26, 2026-11-18) is 16 days out. Zero-compute shortcut to try first: if the RUN_007–RUN_010 logs carry a per-epoch validation curve *and* per-epoch checkpoints or prediction files exist, re-select the val-best epoch and score the test split once there (disclose that the test split was read during training in those runs). If neither route fits before the deadline, C1 cannot be locked as "confirmed" under its own rule — that is the RETHINK branch; report it as an unregistered comparison with the deviation disclosed.
2. **Free.** Relabel the metric. The ledger values are MSE; either publish them as MSE, or convert to the pre-registered primary metric RMSE = sqrt(MSE) (PROTOCOL.md:47) and test the pre-registered statistic — the *RMSE* difference against the 0.010 margin (PROTOCOL.md:36, 42; HYPOTHESES.md:16) — not the MSE difference −0.019 now quoted in C1. On the present (non-P1) rows that is 0.415 ± 0.004 vs 0.437 ± 0.005, difference −0.023. Ridge (C3): 0.298 is MSE; RMSE 0.546. Fix the guard at RESEARCH_STATE.md:31 to match.
3. **Free.** Set HYPOTHESES.md:20 (`outcome: confirmed`) back to pending until change 1 lands; replace EXPERIMENT_LEDGER.md:51 ("Anomalies: none recorded in RUN_001–RUN_010") with the deviation.

## 2. Premise audit

**P1 — What the measured quantity really is: the Quarry-Net test score is a P1 score (checkpoint selected on validation, test read once).**
Quoted: RESEARCH_STATE.md:39 "protocol P1"; RESEARCH_STATE.md:23 "P1 protocol (split, seeds, early-stopping rule) frozen 2026-09-26"; PROTOCOL.md:10-11 "The test split is never read during training or early stopping; it is scored once per run"; PROTOCOL.md:27-29 "Early stopping monitors validation RMSE ... the selected model is the one at the best validation RMSE".
Status: **FALSE.**
Evidence: EXPERIMENT_LEDGER.md:31-34 — config of RUN_007, RUN_008, RUN_009, RUN_010 is `train_nn.py cfg/quarry.yaml --seed N --early_stop_split test --patience 20`; EXPERIMENT_LEDGER.md:17-18 defines the flag: "`--early_stop_split` names the split whose RMSE the patience monitor reads". The four GBT rows (EXPERIMENT_LEDGER.md:26-30) and RUN_011/RUN_012 (:35-36) use `val`. Command run: `grep -c "early_stop_split test"` → 4; `grep -c "early_stop_split val"` → 7; per-row listing confirms the 4 are exactly RUN_007–RUN_010.
Assumed: Quarry-Net checkpoint chosen on validation, test scored once. Found: checkpoint chosen as the test-RMSE-best epoch under patience 20 over ~139–151 epochs (notes, :31-34); the test split was read every epoch.
Corroborating signature (computed from the rows): test − val gap is +0.008, +0.008, +0.008, +0.007 on the four GBT rows (val-stopped) and −0.007, −0.005, −0.007, −0.006 on the four Quarry-Net rows (test-stopped) — the sign follows the monitored split. Caveat, stated honestly: RUN_011 (val-stopped, log target, single seed) also shows −0.006, so part of the Quarry-Net test<val pattern may be a model property rather than selection; this does not alter the status, which rests on the config strings alone.

**P2 — The published quantity is RMSE.**
Quoted: RESEARCH_STATE.md:37 "Quarry-Net reaches RMSE 0.172 ± 0.003 against 0.191 ± 0.004"; :31 guard "'0.172 ± 0.003' is the Quarry-Net test RMSE"; :44 "Ridge regression (RUN_001) reaches test RMSE 0.298"; PROTOCOL.md:47 "Primary: RMSE on the standardised target".
Status: **FALSE.**
Evidence: EXPERIMENT_LEDGER.md:23 column header "test MSE"; :40 "test MSE mean ± sd"; :52-53 "MSE and MAE are computed by `scripts/eval_test.py` on the standardised target from the same predictions file". Arithmetic check (run): RMSE ≥ MAE holds for every error vector; the claimed "RMSE" 0.172 < MAE 0.312, 0.191 < 0.333 and 0.298 < 0.421 are impossible for RMSE on the same predictions; sqrt of the logged values (0.415, 0.437, 0.546) are all ≥ the MAEs, with MAE/RMSE ratios 0.754 / 0.762 / 0.771 (Gaussian reference 0.798) — consistent with the ledger label.
Assumed: RMSE 0.172 / 0.191 / 0.298, difference −0.019 against a 0.010 RMSE margin. Found: MSE 0.172 / 0.191 / 0.298; RMSE 0.4147 / 0.4373 / 0.5459; RMSE difference on these rows −0.0226 (Welch t = −7.36, df = 5.91, p = 3.5e-4). The sign and the margin verdict survive the relabel on the current rows; the published numbers do not.

**P3 — The aggregates, difference and test statistic follow from the per-seed rows.**
Quoted: RESEARCH_STATE.md:37-41 "0.172 ± 0.003 against 0.191 ± 0.004 ... −0.019 ... Welch t-test gives p < 0.001 (df ≈ 5.8)"; :42-43 "MAE is 0.312 ± 0.003 ... 0.333 ± 0.004".
Status: **VERIFIED** (as arithmetic on the logged values; the per-seed values themselves trace to run logs I do not have).
Evidence (python3 output): GBT test column 0.193/0.189/0.196/0.187 → 0.19125 ± 0.00403; Quarry-Net 0.171/0.176/0.168/0.173 → 0.17200 ± 0.00337; diff −0.01925; Welch t = −7.331, df = 5.82, two-sided p = 3.8e-4. MAE: 0.33325 ± 0.00403 vs 0.31250 ± 0.00289 (0.3125 → 0.312 half-even, 0.313 half-up). All match the stated precision. Caveat: correct arithmetic on a non-P1 quantity (P1) with the wrong label (P2).

**P4 — Both learned arms received the same search budget and the same early-stopping budget and rule.**
Quoted: PROTOCOL.md:19-20 "Both learned arms receive the same search budget (40 trials) and the same early-stopping budget (patience 20)"; PROTOCOL.md:15-18 "40-trial random search on the validation split (seed 1)".
Status: patience 20 **VERIFIED** on every learned-arm row (EXPERIMENT_LEDGER.md:26-34, `--patience 20` in each config); same selection *rule* **FALSE** (see P1); 40-trial search on validation for each arm **UNVERIFIED** — no search log or `cfg/*.yaml` is in the evidence set. Check that settles it: the search driver's log or config for `cfg/quarry.yaml`, specifically which split its objective read.

**P5 — The source of record reproduces the table.**
Quoted: RESEARCH_STATE.md:10 "`scripts/report_p1.py` at code_rev `c27e51a`"; :59 "Source of record: `scripts/report_p1.py` at `c27e51a`; it reads the ledger rows above"; PROTOCOL.md:43-44.
Status: **UNVERIFIED** — the script, the code_rev and the run logs are outside the evidence paths (environment note, not a defect). Check: run `scripts/report_p1.py` at `c27e51a` and diff its output against EXPERIMENT_LEDGER.md:42-44; also read whether it (or `scripts/eval_test.py`) emits MSE or RMSE, which decides where the P2 mislabel entered.

Other FALSE premises found (one line each):
- EXPERIMENT_LEDGER.md:51 "Anomalies: none recorded in RUN_001–RUN_010 logs beyond the RUN_004 wall-clock kill" — FALSE as a description of the run set: rows 31-34 carry a protocol deviation in their config.
- HYPOTHESES.md:20 "outcome: confirmed (−0.019, p < 0.001; RESEARCH_STATE C1)" — not established: the rule at :17 was applied to non-P1 rows and to an MSE difference, while :16 states the margin in RMSE.
- RESEARCH_STATE.md:31 guard "'0.172 ± 0.003' is the Quarry-Net test RMSE" — FALSE (it is test MSE); the guard would lock the mislabel into every caption.

## 3. Strongest alternative

**Do nothing (lock as written).** Publishes "RMSE 0.172" for a number that is an MSE, and a primary comparison in which one arm's checkpoint was chosen on the split it is scored on. Both are visible to anyone holding the ledger, because the ledger ships the config strings and its own column headers; a reviewer finds them in minutes. Dominated.

**Cheapest test (≈0 GPU).** Open the RUN_007–RUN_010 logs: if a per-epoch validation curve and per-epoch checkpoints (or prediction files) exist, pick the val-best epoch and score the test split once there, then apply the pre-registered rule on RMSE. Cost: an afternoon. It yields a P1-selected number but not a P1-run (the test split was still read during training), so it must be disclosed. A second free look already computed here: on the *validation* column, where GBT was selected and Quarry-Net was not (a comparison tilted toward GBT), Quarry-Net is still ahead, 0.178 vs 0.184 (MSE), difference −0.005, Welch p = 0.063 — a hint that the P1 gap may be narrower than −0.019 (MSE), not a P1 quantity and not the primary metric.

**Rerun (≈26 GPU-h, HYPOTHESES.md:18).** Four Quarry-Net seeds, `--early_stop_split val`, new rows, pre-registered rule applied on RMSE. The only path to a claim that is literally what H_001 and P1 state.

**Which dominates:** the rerun, entered through the cheap test. If per-epoch checkpoints exist, the cheap test may answer the question for free and the rerun becomes the clean confirmation; if they do not, the rerun is the whole fix. Sixteen days remain; the arm is 4 runs of ~140–150 epochs. Lock after it lands, with the metric relabelled.

## 4. Kill criteria

- **Observable result that stops the lock:** on P1-conformant Quarry-Net rows (val-stopped), the Quarry-Net − GBT **RMSE** difference is > −0.010 or the Welch two-sided p ≥ 0.05 → H_001 is refuted by its own rule (HYPOTHESES.md:17; PROTOCOL.md:35-36) and C1 cannot be locked. In MSE units, against the fixed GBT RMSE mean 0.4373, the Quarry-Net RMSE mean must be ≤ 0.4273, i.e. mean test MSE ≤ 0.1826; the current val-column values of the Quarry-Net rows (0.175–0.181) sit inside that bound but close to it, so the margin is live, not a formality.
- **Cheapest test that could produce it before the full spend:** (i) the zero-compute val-best re-selection from the existing logs, if checkpoints exist; (ii) otherwise one val-stopped seed (~6.5 GPU-h at the project's own rate) as a pilot — if its test MSE lands ≥ 0.183 the full-seed result is at risk. Both are early warnings only: P1 reports all four seeds, and the pilot row stays in the ledger whatever it shows (SCIENTIFIC_RULES §4).

## 5. Regret list

1. Why RUN_007–RUN_010 were launched with `--early_stop_split test` — a launch typo, a default in `cfg/quarry.yaml`, or a code path in `train_nn.py`. If it is a default, RUN_012's row (:36) says `val` but the launch may not; and the hyperparameter search that produced `cfg/quarry.yaml` (PROTOCOL.md:17-18) may have read the test split too, in which case four fresh seeds do not clean the arm.
2. Whether `scripts/eval_test.py` / `scripts/report_p1.py` emit MSE or RMSE — the label error may live in the source of record, and every published number traces to it.
3. Whether the RUN_007–RUN_010 logs hold per-epoch validation curves and checkpoints — decides whether the fix costs 0 or ~26 GPU-h.
4. RUN_004 reached iteration 1,420 before the wall-clock kill, while RUN_005, "identical config" and the same seed (EXPERIMENT_LEDGER.md:28-29, config strings verified identical), early-stopped at 1,360. A seed-deterministic trainer would have stopped RUN_004 at 1,360 first. Either GBT training is not seed-deterministic here (threads, subsampling) or "identical" needs a qualifier; the limitation at RESEARCH_STATE.md:64-65 depends on it.
5. The validation-split gap is only −0.005 (MSE) with p = 0.06 in a comparison tilted toward GBT; nobody has yet said what C1 looks like if the P1 gap comes in at −0.012 rather than −0.019 — still "confirmed" by the rule, but a different paper sentence.

## 6. What is solid

- Row-to-claim mapping is exact: C1 cites RUN_007–RUN_010 vs RUN_002, RUN_003, RUN_005, RUN_006 (RESEARCH_STATE.md:40), matching the complete rows for seeds {1,2,3,4} per arm (EXPERIMENT_LEDGER.md:26-34); the failed RUN_004 row is kept (:28) with its reason; RUN_011 is `pre_reg: false` and in no claim (:35, :50); RUN_012 is `planned` and outside the claim set (:36; RESEARCH_STATE.md:49).
- Arithmetic: means, sample sds, the difference, Welch t/df/p and the MAE aggregates all reproduce from the per-seed values to the stated precision (§2, P3).
- Budget parity: `--patience 20` on every learned-arm row; one frozen config file per arm (`cfg/gbt_tuned.yaml`, `cfg/quarry.yaml`) on every row of that arm.
- RUN_004/RUN_005 config strings are byte-identical (EXPERIMENT_LEDGER.md:28-29).
- On the current rows the relabel does not flip anything: RMSE difference −0.023 against the 0.010 margin, same direction, same significance class; the known-limitations section (RESEARCH_STATE.md:61-69) states n = 4, the relaunch and RUN_011's status correctly.
- Scope claims C3 and C4 make no comparison the ledger cannot support (ridge is a floor; one plant, one feature set).

## 7. Minor

- 0.3125 rounds to 0.312 (half-even) or 0.313 (half-up); state the rounding rule once so the MAE caption is reproducible.
- RESEARCH_STATE.md:55 "0.298 (deterministic)" under an "RMSE" header has the same MSE/RMSE issue as C1; fix in the same pass.
- "df ≈ 5.8" is right (5.82); consider printing the t statistic (−7.33) beside the p-value.
