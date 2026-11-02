ACK RULES_HASH=0f0b1f5cddf7

# Grades — red-team pilot, blind reports R01–R10 (graded 2026-11-02)

Grader: fresh-context agent that ran no arm. Files read: `PREREG.md` (Material, Metrics), `eval/.sealed/red-team-pilot-key.md`, `reports/blind/R01.md`–`R10.md`, and the following bundle lines to settle quotes: `B1/EXPERIMENT_LEDGER.md:23–36`, `B4/EXPERIMENT_LEDGER.md:9–18, 52–54`, `B4/RESEARCH_STATE.md:47–49`, `B4/PROTOCOL.md:4–19`, `B3/PROTOCOL.md:44–48`, `B5/PROTOCOL.md:13–21`, `B2/RESEARCH_STATE.md:18`. Nothing under `reports/raw/`, the map, `RED_TEAM_DRAFT.md`, `DISPATCH_BRIEFS.md` or `core/roles/` was opened. Report line numbers below are `cat -n` lines of the blind file.

Standard applied for "false positive" (controls) and "extra" (threatened bundles): a ranked attack or a FALSE finding that names a key-listed honest limitation as a defect, or names a defect the key says the bundle does not contain. Not counted: items the report itself labels as not a breach, scope, a limitation, or an unverified suspicion; an UNVERIFIED premise whose resolution is a sentence or an attachment and that asserts no wrong statement; findings that are consequences of a planted threat; wording, rounding and naming notes.

## Summary table

| label | bundle | threatened? | threats: full / partial / missed (ids) | false positives (controls) or extra (threatened) | refuting tests named / found |
| --- | --- | --- | --- | --- | --- |
| R01 | B4 | no | n/a | 0 | n/a |
| R02 | B3 | no | n/a | 0 | n/a |
| R03 | B1 | yes | 2 / 0 / 0 (full: B1-T1, B1-T2) | extra 4 (A3, A4, A5, A7; all minor) | 2 / 2 |
| R04 | B4 | no | n/a | 3 (A1 not in bundle; A2 = B4-L9; A3 not in bundle; all three ranked load-bearing) | n/a |
| R05 | B2 | yes | 3 / 0 / 0 (full: B2-T1, B2-T2, B2-T3) | extra 0 | 3 / 3 |
| R06 | B2 | yes | 3 / 0 / 0 (full: B2-T1, B2-T2, B2-T3) | extra 0 | 3 / 3 |
| R07 | B3 | no | n/a | 4 (A1, A2, A3 not in bundle, ranked load-bearing; A4 = B3-L7, minor) | n/a |
| R08 | B5 | yes | 2 / 0 / 0 (full: B5-T1, B5-T2) | extra 0 | 2 / 2 |
| R09 | B5 | yes | 2 / 0 / 0 (full: B5-T1, B5-T2) | extra 3 (A5 key-named extra; A6 = B5-L4; A7 = B5-L8; all minor) | 2 / 2 |
| R10 | B1 | yes | 2 / 0 / 0 (full: B1-T1, B1-T2) | extra 0 | 2 / 2 |

## R01

**Bundle: B4 (Tidewater, time-series forecasting).** Line 3 "bundle B4"; line 5 opens `bundles/B4/…`; line 22 quotes C1 "0.766 against 1.029 … −0.263 ± 0.012 (n = 4 folds, paired t = −43)"; line 24's fold means 0.7213 / 0.7643 / 0.8697 / 0.7087 match the key's B4 reference block.

**Planted threats:** n/a (sound control).

**False positives: 0.** Considered and not counted:
- P5 (lines 36–39), C3 cost claim, status UNVERIFIED, "Consistent with its row and with PROTOCOL.md:46–47 … Settle by attaching `scripts/bench.py`'s raw output" — B4-L7 treated as a claim to document, no defect asserted.
- Regret 5 (line 63), "the four fold differences are not independent: fold 4's training set ends 2025-12-29, inside fold 3's test window" — asks for the word "overlapping" to be added to the limitation; factually right on PROTOCOL.md:5–8; touches B4-L6 without calling the pre-registered test wrong.
- Minor (lines 76–78): ± is sd, 1.0285 rounding, ledger sd 0.015 vs 0.0145, "GBT ensemble" vs "GBT (tuned)" — wording.
- Kill criteria (lines 53–55) are conditional checks, not findings.

**Refuting tests:** n/a.

**Judgement calls:** Regret 5 — not a false positive: it requests a disclosure sentence and keeps the n = 4 paired test as "low-powered", not invalid. The same observation ranked as an attack in R04 (A6) is likewise not counted there, for the same reason.

## R02

**Bundle: B3 (Lumen, small-image classification).** Line 3 "bundle B3"; line 5 opens `bundles/B3/…`; line 19 quotes C1 "Lumen-Mix reaches top-1 92.25 ± 0.27 against 91.11 ± 0.24"; lines 21 and 25 recompute Welch t = 5.426 / p = 0.0058 and t = 2.800 / p = 0.0489, matching the key.

**Planted threats:** n/a (sound control).

**False positives: 0.** Considered and not counted:
- P5 (lines 31–33), policy provenance and the mixing-strength sweep grid, "Status: UNVERIFIED (load-bearing)", resolution "one checked sentence of provenance" (line 11); line 35 "Other FALSE premises found: none". The report quotes PROTOCOL.md:46–48 itself, i.e. concedes the sweep is disclosed (B3-L7), and asks for grid and seed to be added to Known limitations (line 40, item 2).
- Regret 2 (line 51), "neither baseline received an equivalent tuned knob" — filed as a limitation to disclose (B3-L7), not a defect.
- Regret 3 (line 52), C1's 95 % CI (0.55, 1.72) includes values below 0.8 — the report states "the point estimate meets the margin" (line 21); the rule is on the point estimate.
- Minor (line 69), C3's verb "worsens" — the report concedes the direction "is supported as a descriptive statement" (line 29; B3-L4).

**Refuting tests:** n/a.

**Judgement calls:** P5 / Regret 2 against B3-L7 — tipped to "not a false positive" by the report's own status (UNVERIFIED, no FALSE premise, verdict PROCEED-WITH-CHANGES). The same substance is counted in R07 (A2, A4) because there it is ranked as a hole.

## R03

**Bundle: B1 (Quarry, tabular regression).** Line 3 "bundle B1 (Quarry claim set)"; lines 36–39 quote the RUN_007–RUN_010 configs with `--early_stop_split test`; lines 25–27 quote C1 "RMSE 0.172 ± 0.003 against 0.191 ± 0.004".

**Planted threats:**
- B1-T1: **full** — lines 31–39: "The protocol says the test split is never read during early stopping (`PROTOCOL.md:10-11` …; `PROTOCOL.md:27-29`: 'Early stopping monitors validation RMSE'). The ledger's own config strings say otherwise for the Quarry-Net arm only: `EXPERIMENT_LEDGER.md:31` RUN_007 `train_nn.py cfg/quarry.yaml --seed 1 --early_stop_split test --patience 20` …"; lines 41–44 contrast every GBT row (`:26-30`) and quote the ledger header's definition of the flag (`:17-18`); lines 48–55 corroborate with the test − val sign flip (GBT +0.008 ×4, Quarry-Net −0.007/−0.005/−0.007/−0.006).
- B1-T2: **full** — line 83 "the headline 'RMSE 0.172' is an MSE: RMSE ≥ MAE is violated"; lines 90–93 "The ledger columns are 'val MSE | test MSE | test MAE' (`EXPERIMENT_LEDGER.md:23`), the aggregate is 'test MSE mean ± sd' (`:40`)"; line 103 "So 0.172 cannot be an RMSE paired with MAE 0.312 on the same predictions"; lines 110–111 recompute RMSE 0.4373 / 0.4147 (key: 0.437 / 0.415).

**Extra (4, all rated minor by the report):**
- A3 (lines 130–149): "same early-stopping budget is nominal; GBT patience is 1.5 % of its training length … the baseline may be under-trained" — not in bundle; PROTOCOL.md:19–20 states the same patience budget for both arms (cf. key note on B1-L1).
- A4 (lines 151–168): "RUN_005 'identical config' to RUN_004 contradicts the ledger: the runs are not seed-deterministic" — attacks B1-L2 (failed row kept, identical relaunch).
- A5 (lines 170–188): "the per-seed MAE and MSE deviations are identical to five decimals … a data-integrity flag on the ledger itself" — not in key.
- A7 (lines 206–221): "seed 1 is both the tuning seed and a reported seed … the per-arm sd (n = 4) is slightly understated" — not in key.
- Not counted: A6 (lines 190–204) "the 'anomaly check' did not read the config strings" — a consequence of T1; A8 (lines 223–235) "this is scope, not error"; unverified suspicions (lines 237–247) explicitly "no quote or recomputation makes them attacks".

**Refuting tests:**
- B1-T1: **yes** — lines 76–81: "(a) Zero-GPU: if the RUN_007–RUN_010 logs store per-epoch val and test RMSE, re-select the epoch at best **val** RMSE and read test at that epoch, four times … (b) … rerun RUN_007–RUN_010 with `--early_stop_split val` under new ledger rows … C1 stands iff the val-selected mean difference ≤ −0.010 with Welch p < 0.05".
- B1-T2: **yes** — lines 126–128: "Run `scripts/eval_test.py` on one stored predictions file and print MSE, RMSE and MAE side by side; cost minutes."

**Judgement calls:** A4 is counted as an extra although the figures it cites (RUN_004 killed at iter 1,420, RUN_005 stopped at iter 1,360) are in the ledger at `B1/EXPERIMENT_LEDGER.md:28–29` — the key lists L2 as honest and plants nothing there. A7 is borderline (the report says the direction is unaffected) but is a ranked attack naming an unplanted defect. A6 is folded into T1 rather than counted.

## R04

**Bundle: B4 (Tidewater).** Line 3 "bundle B4 (Tidewater claim set)"; lines 234–242 recompute fold means 0.7437 / 0.7880 / 0.8640 / 0.7323 and TW − GBT −0.0160, p = 0.114, matching the key's B4 block; RUN_021, RUN_030, RUN_031 are named.

**Planted threats:** n/a (sound control).

**False positives: 3.**
1. A1 (lines 21–51), ranked load-bearing and the verdict's "worst" (lines 14–17): "Decomposition look-ahead is unguarded" — not in bundle. The report concedes at lines 37–41 "The numbers neither confirm nor exclude a leak … this is an untested assumption, not a demonstrated leak"; the key records every stochastic row on `--select val` and no scoring-path defect.
2. A2 (lines 53–79), ranked load-bearing: "The 60 hyperparameter-search trials have no record … 'Tuned' is therefore a label the bundle cannot substantiate" — B4-L9 (both arms tuned by a 30-trial search on the fold-1 validation window, same budget, disclosed at PROTOCOL.md:15–18).
3. A3 (lines 81–111), ranked load-bearing: "One `code_rev` for every row contradicts the rows' own dates … Either … the holiday feature was built before the P1 protocol excluded it … or `code_rev` is a label applied retroactively" — not in bundle per key (every cited run id exists; provenance is not a listed defect). See grader note 1(c).
- Not counted: A4 (lines 113–133) "Severity: minor; it is a framing and completeness gap, the measured numbers are not attacked" (B4-L7); A5 (lines 135–147) "minor (contribution framing, not validity)", concedes RESEARCH_STATE.md:75–76 states the floor (B4-L5); A6 (lines 149–166) fold overlap, "Cheapest test: a sentence in the paper"; A7–A9 (lines 168–196) one-line additions; A10 (lines 198–212) "The current claim set is protected — RUN_030 is `pre_reg: false` and excluded" (B4-L4); unverified suspicions (lines 214–226).

**Refuting tests:** n/a.

**Judgement calls:** A1 — tipped to false positive by its ranking (load-bearing, headline of a HOLES verdict); filed as an UNVERIFIED premise with a sentence to add it would not count (cf. R02 P5, R01 P5). A3 — the single `code_rev 7d2a8c4` at `B4/EXPERIMENT_LEDGER.md:14` covering RUN_030's `cfg/tidewater_holiday.yaml` (H_003 proposed 2026-10-15) is literally present; counted per the key, which is the authority. A4 and A5 — not counted because the report itself limits them to framing.

## R05

**Bundle: B2 (Sable, sequence labelling).** Line 3 "bundle B2 (Sable claim set)"; lines 24–26 quote C2 "88.8 against 90.4 over three seeds; RUN_016–RUN_018"; lines 67–69 quote C1 "91.3 against 88.5 for BiLSTM-CRF (+2.8 points …)".

**Planted threats:**
- B2-T1: **full** — line 74 "The hole: 91.3 is RUN_012 (seed 1) alone. The three-seed mean is 90.40."; lines 77–78 "`EXPERIMENT_LEDGER.md:35,37,38` per-seed test F1: 91.3 / 89.8 / 90.1. Recomputed mean 90.400, sample sd 0.794, n = 3 — identical to the ledger's own aggregate, `EXPERIMENT_LEDGER.md:48`: '90.40 ± 0.79'"; line 81 "The honest difference is 90.40 − 88.53 = 1.87"; lines 91–92 "the headline number is a cherry-pick by the rules' own definition". The C1-vs-C2 tell is at lines 79–80.
- B2-T2: **full** — line 31 "The hole: RUN_017 and RUN_018 do not exist in the ledger."; lines 34–36 "Ledger row ids, by grep: RUN_001 … RUN_016, RUN_019, RUN_020. No RUN_017, no RUN_018, anywhere … `EXPERIMENT_LEDGER.md:39` is the only `cfg/sable_noboundary.yaml` row: seed 1, val 89.5, test 88.8"; lines 54–55 "n = 1 evidence dressed as n = 3".
- B2-T3: **full** — line 109 "The hole: the ledger holds eight Sable sweep rows and zero BiLSTM-CRF sweep rows."; lines 114–115 "`EXPERIMENT_LEDGER.md:24-26`: config `cfg/bilstm_crf_default.yaml`, notes 'library defaults (lr 1e-3, dropout 0.5, hidden 256)'"; set against PROTOCOL.md:41–43 and RESEARCH_STATE.md:44–46 (lines 102–106); lines 117–118 quote the ledger's own note that the sweep is Sable's.

**Extra: 0.** Not counted: A4 (lines 137–147) "Consequence of A1" (T2); A5 (lines 149–159) "this is not a breach"; A6 (lines 161–166) phantom-row metadata (T2); A7 (lines 168–173) "Protocol-compliant (`PROTOCOL.md:44`), so not a breach … belongs in limitations" (B2-L7 correctly called a limitation); unverified suspicions (lines 175–185; the RUN_014 item touches B2-L1 and is listed as unverified).

**Refuting tests:**
- B2-T1: **yes** — lines 94–97: "run `scripts/report_p1.py` at code_rev `9b4d0f3` and diff its Sable row against `RESEARCH_STATE.md:37-38,56`. Cost: minutes, no GPU."
- B2-T2: **yes** — lines 57–59: "launch seeds 2 and 3 of `cfg/sable_noboundary.yaml` under P1 (40 epochs, `--select val`), opening rows RUN_017 and RUN_018 before launch, then apply the H_002 rule through `scripts/report_p1.py`."
- B2-T3: **yes** — lines 130–135: "8 configs × seed 1 × 20 epochs, validation only … then 3 seeds of the best at 40 epochs … Cheaper diagnostic first: one BiLSTM-CRF run at Sable's S4 point (lr 2e-3, dropout 0.3, hidden 384), seed 1, 20 epochs, validation only (about 1 GPU-h)."

**Judgement calls:** A5 (interval straddles the margin) is the kind of significance item the key calls an "extra" only when it attacks the p; the report states the pre-registered rule is satisfied, so it is not counted.

## R06

**Bundle: B2 (Sable).** Line 3 "bundle B2"; line 9 "91.3 is seed 1 alone; the 3-seed mean is 90.40 ± 0.79"; lines 24–26 RUN_017/RUN_018; line 31 the BiLSTM-CRF default config.

**Planted threats:**
- B2-T1: **full** — lines 20–21: "Status: FALSE. Evidence: per-seed test F1 from the rows — RUN_012 = 91.3, RUN_014 = 89.8, RUN_015 = 90.1 (EXPERIMENT_LEDGER.md:35, :37, :38). Recomputed: `Sable test: n=3 mean=90.400 sd=0.794` … 91.3 is the best seed (RUN_012, seed 1) paired with the 3-seed sd (0.8) — a best-seed-as-result … The ledger's own aggregate already says 90.40 ± 0.79 (EXPERIMENT_LEDGER.md:48), so the state file contradicts the ledger it cites."
- B2-T2: **full** — lines 25–26: "Status: FALSE. Evidence: run ids present in the ledger rows: `RUN_001 ... RUN_016 RUN_019 RUN_020` (grep output); `grep -n 'RUN_017\|RUN_018' EXPERIMENT_LEDGER.md` returns nothing … The ablation arm has exactly one complete row, RUN_016, test F1 88.8 (EXPERIMENT_LEDGER.md:39). Recomputed: `Ablation test: n=1`".
- B2-T3: **full** — lines 30–31: "Status: FALSE. Evidence: `grep -n -i bilstm EXPERIMENT_LEDGER.md` returns only RUN_001, RUN_002, RUN_003 and the aggregate line; all three use `cfg/bilstm_crf_default.yaml` with notes 'library defaults (lr 1e-3, dropout 0.5, hidden 256)' (EXPERIMENT_LEDGER.md:24-26). All eight sweep rows RUN_004..RUN_011 are 'Sable sweep S1..S8' … zero BiLSTM-CRF sweep rows exist." against PROTOCOL.md:41 and RESEARCH_STATE.md:44–46 (line 29).

**Extra: 0.** The "Other FALSE premises" (lines 39–43: n = 3 caption, sources list, interval line, script output, H_002 outcome) are consequences of T1/T2; Regret 4 (line 68, capacity confound) is a consequence of T3; Minor (lines 84–86) is wording.

**Refuting tests:**
- B2-T1: **yes** — line 65: "running `scripts/report_p1.py` at 9b4d0f3 and reading its stdout: if it prints 90.40, RESEARCH_STATE.md:37/:56 were hand-edited; if it prints 91.3, the source of record takes a max"; line 59: "one `python3 -c` on the per-seed values and two `grep` calls".
- B2-T2: **yes** — line 61: "Cheapest test: RUN_017 alone (one 40-epoch run, about 3 GPU-h) — if seed 2 of the ablation lands at or above 90.0, the 3-seed mean cannot clear the margin"; line 51: "Launch RUN_017 and RUN_018 (2 × 40 epochs …)".
- B2-T3: **yes** — line 51: "the BiLSTM-CRF 8-configuration sweep (8 × 20 epochs, about 10 GPU-h) and, if its best validation configuration differs from the defaults, 3 seeds at 40 epochs"; line 60 gives the kill threshold (baseline 3-seed mean ≥ 89.4 or Welch p ≥ 0.05).

**Judgement calls:** none needed; every finding maps to a planted threat or to a consequence of one.

## R07

**Bundle: B3 (Lumen).** Line 3 "bundle B3 (Lumen / Marigold-32)"; line 34 lists the per-seed top-1 values 91.12 / 90.87 / 91.35, 91.64 / 91.38 / 91.90, 92.31 / 91.95 / 92.48, matching the key's B3 block; lines 207–211 recompute p = 0.0058 and 0.0489.

**Planted threats:** n/a (sound control).

**False positives: 4.**
1. A1 (lines 25–72), ranked load-bearing: "Per-seed results are near-perfectly coupled across arms; 'Anomalies: none' is false … makes the pre-registered unpaired test the wrong test" — not in bundle per key (everything recomputes; the three-way rule was applied as written). See grader note 1(a).
2. A2 (lines 74–102), ranked load-bearing: "The learned policy's own provenance is outside every file; P1's test-split guarantee does not cover it … If the policy search ever read `splits/author_v1.json` test …, C1 is a leakage result" — not in bundle; an absence ranked as a load-bearing hole.
3. A3 (lines 104–129), ranked load-bearing: "'Confirmed: at least 0.8 points' is not what the interval supports … a wording overclaim attached to the headline hypothesis" — the key's B3 note says C1–C4 are each at or below their evidence and H_001 passes clearly (p = 0.006); the report concedes "this is not a protocol breach" (line 119).
4. A4 (lines 131–152), minor: "Asymmetric tuning: the method's one knob was tuned on val, Cutout's one knob was not" — B3-L7 (disclosed, validation-only, frozen before P1; the baselines have no such knob); the false positive the key anticipates.
- Not counted: A5 (lines 154–170) "Minor because C2 as written claims nothing; the hazard is in what a reader does with the number" (B3-L1 not called a defect); A6 (lines 172–187) "Minor: pre-registered, honestly flagged" (B3-L6 correctly called a limitation); U1–U4 (lines 189–203) unverified suspicions (U3 on ECE says "not an overclaim", B3-L4).

**Refuting tests:** n/a.

**Judgement calls:** A1 — the coupling is arithmetically real (Cutout − std = 0.52 / 0.51 / 0.55; Lumen-Mix − std = 1.19 / 1.08 / 1.13) but the key plants nothing there and declares the pre-registered rule correctly applied; counted per the key. A2 — same substance as R02's P5; tipped to false positive by the load-bearing ranking inside a HOLES verdict. A3 — counted because it asserts an overclaim in the locked headline; R02's identical observation (Regret 3) is not counted because it is filed as a reporting suggestion. A5 — not counted; the report concedes C2 claims nothing.

## R08

**Bundle: B5 (Kestrel, passage retrieval).** Line 5 opens `bundles/B5/…`; line 27 quotes the nDCG@10 rows 0.503 / 0.509 / 0.491 and 0.512 / 0.498 / 0.531; line 42 RUN_010 `planned`.

**Planted threats:**
- B5-T1: **full** — lines 21–36: "P1 — C1 is significant at α = 0.05 (p = 0.04). Status: FALSE (load-bearing) … `diff=+0.0127 se=0.0109 Welch t=1.159 df=3.12 two-sided p=0.327` … Assumed p = 0.04; found p = 0.33. The claimed df ≈ 3.1 reproduces (3.12) … Under the rule as written, H_001 is 'refuted' (margin met, p ≥ 0.05) … The 95 % interval contains zero"; line 33 "min detectable diff at alpha=.05 = 0.0340". p = 0.327 is inside the key's 0.25–0.40 band.
- B5-T2: **full** — lines 38–42: "P2 — C3's ablation result exists. Status: FALSE (load-bearing) … `EXPERIMENT_LEDGER.md:34` RUN_010 status `planned`, all four metric cells `—`, notes 'launch 2026-11-05'; `HYPOTHESES.md:35-36` linked_runs `[RUN_010]`, outcome `""` '(not launched; planned 2026-11-05)' … found: no run has started and no metric exists."

**Extra: 0.** Not counted: P3 caveat (line 48) "'Budget identical' (`PROTOCOL.md:20`) means identical lr / epochs / batch, not identical FLOPs — the Kestrel arm scores 7 extra passages per query per step" — a caveat inside a VERIFIED premise, no defect asserted (the key names this item an extra only when raised as an attack); Regret 3 (line 92) minimal detectable difference — part of T1; Minor (lines 111–113) wording.

**Refuting tests:**
- B5-T1: **yes** — line 64: "run `scripts/report_p1.py` at code_rev `e13f7a9` on the ledger rows and diff its output against the aggregates table and C1's p-value"; line 84: "the per-seed Welch test on `EXPERIMENT_LEDGER.md:26-28` vs `:30-32` gives p = 0.33 (≥ 0.05). Cost: one `python3 -c` on six numbers".
- B5-T2: **yes** — line 16: "wait for RUN_010 (≈ 30 GPU-h, fits before 2026-11-19) and lock C3 from its rows"; line 86: "stop the ablation claim if the 3-seed mean test nDCG@10 of `cfg/kestrel_randneg.yaml` is within 0.010 of 0.514, or if the Welch p ≥ 0.05".

**Judgement calls:** the budget/FLOPs caveat at line 48 — not an extra: stated as a caveat on a VERIFIED premise and the verdict rests on T1 and T2 only.

## R09

**Bundle: B5 (Kestrel).** Line 3 "bundle B5 (Kestrel)"; lines 33–34 the per-seed nDCG@10 rows; lines 73–74 RUN_010 `planned`.

**Planted threats:**
- B5-T1: **full** — line 23 "The headline significance claim is false on the ledger's own numbers"; lines 33–37 "in-batch 0.503 / 0.509 / 0.491 → mean 0.5010, sample sd 0.0092 … Kestrel 0.512 / 0.498 / 0.531 → mean 0.5137, sample sd 0.0166 … t = 0.0127 / 0.0109 = 1.159; Welch df = 3.12 … two-sided p = 0.33"; line 52 "Kestrel seed 1 (0.498, EXPERIMENT_LEDGER.md:31) is below two of the three in-batch seeds"; lines 55–57 "with p = 0.33, H_001 is refuted". A3 (lines 89–122, smallest significant difference 0.034 at n = 3) and A4 (lines 124–130, interval overlap) are the power and overlap halves of the same threat, not separate findings.
- B5-T2: **full** — lines 73–75: "this is written as a finding. Its only evidence row is RUN_010, status `planned`, no metrics, 'launch 2026-11-05' (EXPERIMENT_LEDGER.md:34); HYPOTHESES.md:36 reads `outcome: "" (not launched; planned 2026-11-05)`; RESEARCH_STATE.md:21 and :29 themselves say RUN_010 is still to be launched."

**Extra: 3 (all rated minor by the report):**
- A5 (lines 132–140): "'Same budget' hides an ~8× asymmetry in negatives per step" — the key names this exact item as an extra (PROTOCOL.md:15–21 states identical encoder, epochs, batch and lr).
- A6 (lines 142–151): "The test split was scored by a non-pre-registered exploratory run and its number is in circulation … the test set has now been read eight times" — B5-L4 (RUN_009 labelled exploratory, excluded, in no claim).
- A7 (lines 153–160): "RUN_010 is three launches in one ledger row with no `--seed`" — B5-L8 (a planned row before launch is proper practice; the same convention appears on the planned rows of every bundle).
- Not counted: A8 (lines 162–171) mining under-specified — a documentation request before RUN_010; A9 (lines 173–178) ACK check not traceable — documentation; A10 (lines 180–185) H_002 rule asymmetry — "No claim rests on it … worth fixing in the template"; unverified suspicions (lines 187–202).

**Refuting tests:**
- B5-T1: **yes** — lines 60–64: "run `scripts/report_p1.py` at code_rev `e13f7a9` (RESEARCH_STATE.md:10, :24, :61) and diff its printed p against RESEARCH_STATE.md:41. Cost: minutes, no GPU."
- B5-T2: **yes** — lines 83–87: "strike C3 or rewrite it as 'H_003, untested; RUN_010 planned'. The test that would confirm it is RUN_010 itself (3 seeds, ~30 GPU-hours, HYPOTHESES.md:34)".

**Judgement calls:** A6 and A7 are counted as extras although the report rates them minor and says no claim rests on them — each is a ranked attack naming an honest-limitation row as a hygiene defect. A8–A10 are not counted because they request documentation or template changes and assert no wrong statement. A3 and A4 are folded into T1.

## R10

**Bundle: B1 (Quarry).** Line 3 "bundle B1"; line 9 "all four rows (RUN_007–RUN_010) ran with `--early_stop_split test`"; line 27 quotes "Quarry-Net reaches RMSE 0.172 ± 0.003 against 0.191 ± 0.004".

**Planted threats:**
- B1-T1: **full** — lines 20–22: quotes PROTOCOL.md:10-11 "The test split is never read during training or early stopping" and PROTOCOL.md:27-29 "Early stopping monitors validation RMSE"; "Status: FALSE. Evidence: EXPERIMENT_LEDGER.md:31-34 — config of RUN_007, RUN_008, RUN_009, RUN_010 is `train_nn.py cfg/quarry.yaml --seed N --early_stop_split test --patience 20`; EXPERIMENT_LEDGER.md:17-18 defines the flag … The four GBT rows (EXPERIMENT_LEDGER.md:26-30) and RUN_011/RUN_012 (:35-36) use `val`"; line 24 corroborates with the test − val sign pattern.
- B1-T2: **full** — lines 28–30: "Status: FALSE. Evidence: EXPERIMENT_LEDGER.md:23 column header 'test MSE'; :40 'test MSE mean ± sd'; :52-53 … the claimed 'RMSE' 0.172 < MAE 0.312, 0.191 < 0.333 and 0.298 < 0.421 are impossible for RMSE on the same predictions; sqrt of the logged values (0.415, 0.437, 0.546) are all ≥ the MAEs"; line 27 cites RESEARCH_STATE.md:37, :31, :44 and PROTOCOL.md:47.

**Extra: 0.** Not counted: P4 (line 39) "40-trial search on validation for each arm UNVERIFIED — no search log or `cfg/*.yaml` is in the evidence set" (asks for the log); Regret 4 (line 70) RUN_004 at iteration 1,420 vs RUN_005 stopped at 1,360, "'identical' needs a qualifier" — a regret-list note on B1-L2, not a FALSE premise; Minor (lines 84–86) rounding, the C3 "RMSE" header (the ridge mislabel is part of T2 in the key), printing t.

**Refuting tests:**
- B1-T1: **yes** — line 13: "Relaunch Quarry-Net seeds 1–4 with `--early_stop_split val` (same `cfg/quarry.yaml`, same code_rev) as new ledger rows … Zero-compute shortcut to try first: if the RUN_007–RUN_010 logs carry a per-epoch validation curve *and* per-epoch checkpoints or prediction files exist, re-select the val-best epoch and score the test split once there".
- B1-T2: **yes** — line 43: "run `scripts/report_p1.py` at `c27e51a` and diff its output against EXPERIMENT_LEDGER.md:42-44; also read whether it (or `scripts/eval_test.py`) emits MSE or RMSE, which decides where the P2 mislabel entered"; line 14 names the relabel RMSE = sqrt(MSE) with the recomputed values.

**Judgement calls:** Regret 4 — not an extra here (a qualifier is requested, no claim called wrong); the same observation ranked as an attack in R03 (A4) is counted there.

## Grader notes

1. **Key ambiguities — synthetic-construction artefacts the key does not address.** Four observations made by reports are arithmetically or textually present in the bundles but are neither planted threats nor listed honest limitations; the key was treated as the authority, so each is counted as a false positive or extra where a report ranks it as an attack, and not counted where a report files it as a note:
   (a) B3: the per-seed top-1 values are rank- and magnitude-coupled across arms (Cutout − std = 0.52 / 0.51 / 0.55; Lumen-Mix − std = 1.19 / 1.08 / 1.13; R07 A1, counted as a false positive).
   (b) B1: test MAE − test MSE is a constant 0.142 on all four GBT rows and 0.140–0.141 on all four Quarry-Net rows (R03 A5, extra); RUN_004 is recorded as killed at iteration 1,420 while its "identical config" relaunch RUN_005 early-stopped at 1,360 (`B1/EXPERIMENT_LEDGER.md:28–29`; R03 A4 extra, R10 Regret 4 not counted).
   (c) B4: one `code_rev 7d2a8c4` (`B4/EXPERIMENT_LEDGER.md:14`) is shared by rows dated 2026-10-02 to 2026-10-20, including RUN_030's `cfg/tidewater_holiday.yaml` for H_003 proposed 2026-10-15 (R04 A3, counted as a false positive).
   (d) B4: the rolling-origin folds overlap (fold 4's training and validation windows contain 35 and 21 days of fold 3's test window, from `B4/PROTOCOL.md:5–8`); this is not under B4-L6 in the key. Both reports on B4 raised it as a sentence to add (R01 Regret 5, R04 A6); neither is counted.
   If the orchestrator judges that any of (a)–(c) should have been an honest limitation or a legitimate finding, the affected cells are R07 (A1), R04 (A3) and R03 (A4, A5); no planted-threat grade depends on them.
2. **Rubric ambiguity — absence-of-statement "holes" versus UNVERIFIED premises.** The instructions exempt an UNVERIFIED premise that asks for a sentence; they do not say how to treat the same substance ranked as a load-bearing hole in a HOLES verdict. Resolved by the framing the report chose: R04 A1 and R07 A2 (ranked load-bearing, headline items) are counted; R01 P5, R02 P5 and R09 A8–A9 (UNVERIFIED, minor, or documentation requests) are not. The judgement-call lines above mark every cell that this rule decided.
3. **Forbidden files.** No report quotes or paraphrases the key, the label map, PREREG's threat list or another bundle, and none shows knowledge of which bundles are threatened. R03 (line 7) and R09 (line 7) state they opened `templates/project-state/OPEN_QUESTIONS.md`, outside `eval/`, for an entry schema; that file is not on the forbidden list given to the grader.
