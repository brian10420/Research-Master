ACK RULES_HASH=0f0b1f5cddf7

# Red-team report — bundle B1 (Quarry claim set, state_as_of 2026-10-27)

Reviewer: red-team (pilot contract). Inputs opened: `bundles/B1/RESEARCH_STATE.md`,
`bundles/B1/EXPERIMENT_LEDGER.md`, `bundles/B1/HYPOTHESES.md`, `bundles/B1/PROTOCOL.md`
(plus `templates/project-state/OPEN_QUESTIONS.md` outside `eval/` for the entry schema).
Nothing else under `eval/` was opened. Every number below is recomputed from the
per-seed ledger rows (`EXPERIMENT_LEDGER.md:25-35`); no summary number from any of
the four files is used as evidence. Paths are relative to
`eval/pilots/red-team-2026-11-02/bundles/B1/`.

## 1. Verdict

HOLES: 2 load-bearing, 6 minor — worst: all four Quarry-Net checkpoints were selected
on the **test** split (`--early_stop_split test`, `EXPERIMENT_LEDGER.md:31-34`) while the
GBT arm was selected on `val`, so C1's −0.019 is a test-selected number compared against
a clean one, and the honest effect is bracketed by [−0.005, −0.019], which straddles the
pre-registered margin of −0.010.

## 2. Ranked attacks

### A1 — load-bearing — Quarry-Net early-stopped on the test split; GBT on val

**Claim attacked.** `RESEARCH_STATE.md:37-41`: "C1 (H_001, confirmed). On Kiln-Yield
test, Quarry-Net reaches RMSE 0.172 ± 0.003 against 0.191 ± 0.004 … The difference,
−0.019, meets the pre-registered margin (0.010)". Also `RESEARCH_STATE.md:23`: "P1
protocol (split, seeds, early-stopping rule) frozen", and `EXPERIMENT_LEDGER.md:51`:
"Anomalies: none recorded in RUN_001–RUN_010 logs".

**The hole.** The protocol says the test split is never read during early stopping
(`PROTOCOL.md:10-11`: "The test split is never read during training or early stopping";
`PROTOCOL.md:27-29`: "Early stopping monitors validation RMSE"). The ledger's own
config strings say otherwise for the Quarry-Net arm only:

- `EXPERIMENT_LEDGER.md:31` RUN_007 `train_nn.py cfg/quarry.yaml --seed 1 --early_stop_split test --patience 20`
- `EXPERIMENT_LEDGER.md:32` RUN_008 `… --seed 2 --early_stop_split test …`
- `EXPERIMENT_LEDGER.md:33` RUN_009 `… --seed 3 --early_stop_split test …`
- `EXPERIMENT_LEDGER.md:34` RUN_010 `… --seed 4 --early_stop_split test …`

versus every GBT row (`EXPERIMENT_LEDGER.md:26-30`) and the exploratory RUN_011
(`:35`): `--early_stop_split val`. The ledger header defines the flag:
`EXPERIMENT_LEDGER.md:17-18` "`--early_stop_split` names the split whose RMSE the
patience monitor reads. Metrics are scored at the selected iteration or epoch." So the
Quarry-Net test MSE is the minimum of a noisy per-epoch test curve (patience 20), i.e.
a selection-biased statistic, while the GBT test MSE is a single unselected read.

**Corroborating recomputation (test − val, per seed, from `:26-34`).**

| arm | selection split | test − val per seed |
| --- | --- | --- |
| GBT | val | +0.008, +0.008, +0.008, +0.007 |
| Quarry-Net | test | −0.007, −0.005, −0.007, −0.006 |

The sign flips 8/8: each arm scores better on the split it selected on. (Caveat, stated
honestly: RUN_011, a val-selected Quarry-Net with a log target, single seed, also shows
test − val = −0.006, so part of the flip may be an arm × split-difficulty interaction
rather than selection bias. One exploratory run with a different target transform cannot
settle it; the config strings are the direct evidence.)

**How much of C1 is at risk.** Same-split comparisons bracket the honest effect:

- test/test (biased toward Quarry-Net): 0.17200 − 0.19125 = **−0.01925**, Welch t = −7.33, df = 5.82, p = 3.8e−4
- val/val (biased toward GBT, because val is GBT's selection split): 0.17825 − 0.18350 = **−0.00525**, Welch t = −2.35, df = 5.27, p = 0.063
- clean-vs-clean across splits (GBT test vs Quarry-Net val): −0.01300, p = 2.7e−3
- Quarry-Net val + the GBT-observed selection gap (+0.00775) = 0.18600 vs GBT test 0.19125: −0.00525

The pre-registered decision rule (`HYPOTHESES.md:17`: "confirmed iff mean difference
<= −0.010 and Welch p < 0.05") is met at one end of the bracket and fails at the other.
"Confirmed" is therefore not established by the rows as they stand, and the ledger's
"Anomalies: none" (`:51`) is false on its own evidence.

**Severity.** Load-bearing: it is the primary claim, and the protocol freeze
(`RESEARCH_STATE.md:23`) and the submission lock (`:20`) both rest on it.

**Cheapest test.** (a) Zero-GPU: if the RUN_007–RUN_010 logs store per-epoch val and
test RMSE, re-select the epoch at best **val** RMSE and read test at that epoch, four
times; cost ≈ 1 hour of analyst time. (b) If the logs hold only the selected epoch, rerun
RUN_007–RUN_010 with `--early_stop_split val` under new ledger rows; cost ≈ 26 GPU-hours
(`HYPOTHESES.md:18`). Decision: C1 stands iff the val-selected mean difference ≤ −0.010
with Welch p < 0.05, exactly as pre-registered.

### A2 — load-bearing — the headline "RMSE 0.172" is an MSE: RMSE ≥ MAE is violated

**Claim attacked.** `RESEARCH_STATE.md:31`: "`0.172 ± 0.003` is the Quarry-Net test
RMSE"; `:37-38`: "RMSE 0.172 ± 0.003 against 0.191 ± 0.004"; `:53-57` table header
"RMSE (mean ± sd)"; `HYPOTHESES.md:16` and `PROTOCOL.md:47` define the primary metric as
RMSE.

**The hole.** The ledger columns are "val MSE | test MSE | test MAE"
(`EXPERIMENT_LEDGER.md:23`), the aggregate is "test MSE mean ± sd" (`:40`), and the note
says "MSE and MAE are computed by `scripts/eval_test.py` on the standardised target from
the same predictions file" (`:52-53`; same in `PROTOCOL.md:49`). For any error vector,
RMSE ≥ MAE (Jensen / Cauchy–Schwarz on E|e| ≤ sqrt(E e²)). Every row has
"test MSE" < "test MAE":

| arm | column value | MAE | column < MAE | sqrt(column) | MAE / sqrt(column) |
| --- | --- | --- | --- | --- | --- |
| ridge | 0.298 | 0.421 | yes | 0.546 | 0.77 |
| GBT (4 seeds) | 0.187–0.196 | 0.329–0.338 | yes ×4 | 0.432–0.443 | 0.76 |
| Quarry-Net (4 seeds) | 0.168–0.176 | 0.309–0.316 | yes ×4 | 0.410–0.420 | 0.75 |

So 0.172 cannot be an RMSE paired with MAE 0.312 on the same predictions. The ledger
columns are internally consistent as **MSE** (sqrt(MSE) ≥ MAE in all nine rows, ratio
0.75–0.77 against the Gaussian 0.80); the claims file relabels them as RMSE. Either the
claims mislabel MSE as RMSE, or the MAE column is on a different scale than the ledger and
protocol state — both are a wrong published number.

**Recomputation on the RMSE scale (sqrt of each per-seed MSE).**
GBT RMSE 0.4393 / 0.4347 / 0.4427 / 0.4324 → 0.4373 ± 0.0046;
Quarry-Net RMSE 0.4135 / 0.4195 / 0.4099 / 0.4159 → 0.4147 ± 0.0041;
difference −0.0226, Welch t = −7.36, df = 5.91, p = 3.5e−4. Relative improvement is
5.2 % in RMSE, not the 10.1 % the MSE pair implies. The pre-registered margin of 0.010
(`PROTOCOL.md:42`: "superiority margin 0.010") was set on the RMSE scale; on that scale
it is met by the as-ledgered rows, so the *outcome* of H_001 survives this attack — but
every published number (0.172, 0.191, ± 0.003, ± 0.004, −0.019, "df ≈ 5.8"), the guard
line at `RESEARCH_STATE.md:31`, and the result table at `:53-57` are on the wrong scale.
Note also that `scripts/report_p1.py` is cited as the source of record for the RMSE
numbers (`RESEARCH_STATE.md:10, 59`) while the ledger says it aggregates MSE (`:38-44`);
one of the two descriptions of the script is wrong.

**Severity.** Load-bearing for the manuscript: the primary numbers are mislabeled and
the claimed gain is overstated 2× in relative terms. Not load-bearing for the H_001
decision as such (margin still met on the correct scale, before A1 is applied).

**Cheapest test.** Run `scripts/eval_test.py` on one stored predictions file and print
MSE, RMSE and MAE side by side; cost minutes. Then fix the label in whichever file is wrong
and regenerate the table from `scripts/report_p1.py`.

### A3 — minor — "same early-stopping budget" is nominal; GBT patience is 1.5 % of its training length

**Claim attacked.** `PROTOCOL.md:19-20`: "Both learned arms receive the same search
budget (40 trials) and the same early-stopping budget (patience 20)."

**The hole.** Patience 20 counts boosting iterations for GBT and epochs for Quarry-Net
(`EXPERIMENT_LEDGER.md:16-17`). GBT stopped at iterations 1,310 / 1,280 / 1,360 / 1,250
(`:26-30`); Quarry-Net at epochs 143 / 151 / 139 / 147 (`:31-34`). Patience 20 is
≈ 1.5 % of a GBT run and ≈ 14 % of a Quarry-Net run. A 20-iteration patience on a
noisy validation curve stops boosting at the first local plateau; the baseline may be
under-trained. "Same budget" in trials and in patience count is not the same budget in
compute either: `HYPOTHESES.md:18` gives 26 GPU-hours vs 9 CPU-hours.

**Severity.** Minor on its own; it becomes load-bearing if the A1 rerun lands within
≈ 0.005 of the margin, because GBT needs to drop only from 0.191 to 0.182 (MSE) to
break the margin.

**Cheapest test.** Rerun the four GBT seeds with patience 200 (same `cfg/gbt_tuned.yaml`)
under new rows; cost ≈ 9 CPU-hours. C1 is safe from this attack iff the GBT mean
does not fall below 0.182 MSE.

### A4 — minor — RUN_005 "identical config" to RUN_004 contradicts the ledger: the runs are not seed-deterministic

**Claim attacked.** `RESEARCH_STATE.md:64-65`: "RUN_004 … was killed by the scheduler
wall-clock limit at iteration 1,420 and relaunched as RUN_005 with an identical config";
`EXPERIMENT_LEDGER.md:29`: "identical config to RUN_004; stopped at iter 1,360".

**The hole.** RUN_004 was still training at iteration 1,420 (`:28`), so patience-20
early stopping had not fired by 1,420. RUN_005, same seed 3 and "identical config", fired
at 1,360. A deterministic trajectory cannot do both. Either GBT training is
non-deterministic across launches (thread scheduling, data order) or the config was not
identical. The seed-to-seed sd (0.004) that feeds the Welch test then also contains
launch-to-launch noise of unknown size, and "seed" does not pin a run.

**Severity.** Minor (does not change the direction), but it is a reproducibility claim
a reviewer can falsify from the ledger alone.

**Cheapest test.** Relaunch seed 3 GBT once more (≈ 2.3 CPU-hours) and compare the
stopping iteration and test MSE to RUN_005; or diff the two run logs' config dumps.

### A5 — minor — the per-seed MAE and MSE deviations are identical to five decimals

**Claim attacked.** `RESEARCH_STATE.md:51`: "per-seed values in the ledger";
`EXPERIMENT_LEDGER.md:52-53`: "MSE and MAE are computed by `scripts/eval_test.py` … from
the same predictions file".

**The hole (recomputation).** GBT MAE − MSE per seed = 0.142, 0.142, 0.142, 0.142
(`:26,27,29,30`); the GBT MAE deviations from their mean (+0.00175, −0.00225, +0.00475,
−0.00425) equal the MSE deviations exactly. Quarry-Net: 0.141, 0.140, 0.141, 0.140.
Two different functionals of the same residual vector moving in lock-step by a constant
across four independent seeds is not what real predictions produce; it is what a
transcription (one column derived from the other) or a synthetic table produces.

**Severity.** Minor as an attack on the direction of C1, but it is a data-integrity flag
on the ledger itself; if the MAE column was derived rather than measured, C2
(`RESEARCH_STATE.md:42-43`) has no independent evidence.

**Cheapest test.** Re-run `scripts/eval_test.py` on the eight stored predictions files
and confirm the MAE column; cost minutes.

### A6 — minor — the "anomaly check" did not read the config strings

**Claim attacked.** `EXPERIMENT_LEDGER.md:51`: "Anomalies: none recorded in
RUN_001–RUN_010 logs beyond the RUN_004 wall-clock kill"; `:13-14`: "ack line present as
line 1 of every run log (checked 2026-10-27)".

**The hole.** A1's protocol violation is visible in the ledger's own config column; a
check that passed it was checking ACK lines, not protocol conformance. The ACK check is
unverifiable from the bundle (no logs are included).

**Severity.** Minor (process), but it means the "frozen protocol" assurance at
`RESEARCH_STATE.md:23` is asserted, not enforced.

**Cheapest test.** `grep -c 'early_stop_split test'` over the run configs of every
`pre_reg: true` row; cost seconds. Add it to the guardian's check.

### A7 — minor — seed 1 is both the tuning seed and a reported seed

**Claim attacked.** `PROTOCOL.md:15-18`: "40-trial random search on the validation split
(seed 1)" for both arms; `PROTOCOL.md:23`: seeds {1, 2, 3, 4} reported.

**The hole.** The configuration was chosen to look best at seed 1 on val, and seed 1 is
then one quarter of the reported sample. Symmetric across arms, so the direction is not
affected; the per-arm sd (n = 4) is slightly understated. Also unknown from the bundle:
whether the Quarry-Net search trials themselves early-stopped on `test` (if
`--early_stop_split test` was the default in `cfg/quarry.yaml`'s search, the
*configuration* is test-selected, not just the checkpoints).

**Severity.** Minor; upgrades to part of A1 if the search also used the test split.

**Cheapest test.** Read the search driver's config for the Quarry-Net sweep; cost
minutes. Report seeds {2, 3, 4} alongside {1, 2, 3, 4} as a sensitivity line.

### A8 — minor — one baseline, no ablation; the claim is narrow enough, the paper will not be

**Claim attacked.** `RESEARCH_STATE.md:7`: "gated feature-token MLP"; `:69`: "the
comparison target is the tuned GBT".

**The hole.** Nothing in the bundle separates "a neural network beats GBT here" from
"gating / feature tokens beat GBT here". A reviewer of a paper about Quarry-Net will ask
for an ungated MLP at the same budget. The claim set as written does not claim the
mechanism, so this is scope, not error.

**Severity.** Minor.

**Cheapest test.** One ungated-MLP arm, 4 seeds, val-selected; cost ≈ 26 GPU-hours.

### Unverified suspicions (no quote or recomputation makes them attacks)

- `PROTOCOL.md:7`: val and test are exactly 7,218 rows each under a by-batch grouped
  split. Equal counts under grouping are possible but unusual; worth one `wc` on the
  split file.
- `PROTOCOL.md:4-7`: the target is "yield fraction per batch" and rows are grouped by
  batch, so rows within a batch share a target; the effective test size is the number of
  batches, not 7,218, and the split is random-by-batch, not temporal. Does not bias the
  arm comparison; it does bound what "generalisation" means for C4.
- `RESEARCH_STATE.md:18`: DEC_004 is cited but not in the bundle; the freeze date cannot
  be checked.

## 3. What survived

- **Arithmetic of the aggregates.** Means and sample sds at `EXPERIMENT_LEDGER.md:43-44`
  recompute exactly from the rows: GBT 0.19125 ± 0.00403, Quarry-Net 0.17200 ± 0.00337
  (column scale); MAE 0.33325 ± 0.00403 and 0.31250 ± 0.00289. The Welch statistic at
  `RESEARCH_STATE.md:41` recomputes to t = −7.33, df = 5.82, p = 3.8e−4 ("p < 0.001,
  df ≈ 5.8" is correct on the rows as given).
- **Reporting discipline.** All four seeds per arm are reported; the failed RUN_004 keeps
  its row with a reason (`EXPERIMENT_LEDGER.md:28, 48-49`); RUN_011 is marked
  `pre_reg: false`, single seed, and enters no claim (`:35, 50`; `HYPOTHESES.md:33, 36`;
  `RESEARCH_STATE.md:66-68`); RUN_012 is planned, outside the claim set
  (`RESEARCH_STATE.md:21, 49`). No cherry-picking of seeds is visible.
- **Pre-registration shape.** H_001 states the margin, the test, the direction and the
  budget before the runs (`HYPOTHESES.md:14-19`, dated 2026-09-22 vs runs from
  2026-10-01); the decision rule in `HYPOTHESES.md:17` matches `PROTOCOL.md:35-36` and
  `:41-42`.
- **C3 and C4 scoping.** Ridge is explicitly a floor with no claim attached
  (`RESEARCH_STATE.md:44-46`, `PROTOCOL.md:38`); C4 confines the claim to one plant, one
  feature set, one budget (`RESEARCH_STATE.md:47-49`). Both withstand attack as written.
- **C2 direction.** On the ledger rows the MAE difference is −0.0208 (t = −8.37,
  df = 5.44, p = 2.6e−4), agreeing in direction with C1 — conditional on A5 (MAE column
  independently measured) and A1 (same test-selection applies to MAE at the selected
  epoch).
- **Split arithmetic.** 33,684 + 7,218 + 7,218 = 48,120 and the fractions are exactly
  0.70 / 0.15 / 0.15 (`PROTOCOL.md:7`).

## 4. Drafted `OPEN_QUESTIONS.md` entries (returned, never written)

```yaml
id: Q_001
title: Does Quarry-Net still beat tuned GBT by the pre-registered margin when its checkpoints are selected on the validation split, as P1 requires?
date: 2026-11-02
proposer: other-model:red-team-pilot
status: proposed
decided_by: ""
evidence: [EXPERIMENT_LEDGER.md:31-34 (--early_stop_split test on RUN_007-RUN_010), PROTOCOL.md:10-11 and 27-29, same-split bracket val/val -0.00525 (p 0.063) vs test/test -0.01925 (p 3.8e-4)]
why_it_matters: C1 and the H_001 outcome "confirmed" rest on a test-selected number against a val-selected one; the margin -0.010 lies inside the bracket.
what_would_answer_it: Re-select the epoch at best val RMSE from the stored per-epoch curves of RUN_007-RUN_010 and read test there (zero GPU), or rerun the four seeds with --early_stop_split val under new rows (about 26 GPU-hours); apply the H_001 rule unchanged (mean diff <= -0.010 and Welch p < 0.05).
blocked_by: [per-epoch logs of RUN_007-RUN_010, or 26 GPU-hours]
spawned: []
answer: ""
supersedes: ""
superseded_by: ""
```

```yaml
id: Q_002
title: Is the primary metric in the claims MSE or RMSE, and which file (claims or ledger) carries the wrong label?
date: 2026-11-02
proposer: other-model:red-team-pilot
status: proposed
decided_by: ""
evidence: [RESEARCH_STATE.md:31 and 37-38 call 0.172 an RMSE; EXPERIMENT_LEDGER.md:23, 40, 52-53 call the same column MSE; RMSE >= MAE is violated in all nine rows (0.172 < 0.312), consistent only with the column being MSE; on the RMSE scale the difference is -0.0226 (0.4147 vs 0.4373), relative gain 5.2 % not 10.1 %]
why_it_matters: Every published primary number, the RESEARCH_STATE guard line and the result table are on the wrong scale, and the pre-registered margin was set on the RMSE scale.
what_would_answer_it: Run scripts/eval_test.py on one stored predictions file and print MSE, RMSE and MAE side by side; then regenerate the table from scripts/report_p1.py with the correct label and re-state C1 on the RMSE scale.
blocked_by: [stored predictions files, scripts/eval_test.py]
spawned: []
answer: ""
supersedes: ""
superseded_by: ""
```

---
Contract compliance: read-only (cat -n, python3 -I arithmetic, mkdir of the return
directory only); no file under review edited; no training; no pending readout computed
(RUN_012 untouched); no outcome computed on a protected split beyond re-aggregating the
test numbers the ledger already publishes. This report is advice, not a veto.
