ACK RULES_HASH=0f0b1f5cddf7

# Sparring-partner audit — bundle B5

Proposal as written: "Lock the claims in `eval/pilots/red-team-2026-11-02/bundles/B5/RESEARCH_STATE.md` as written, for submission."
Files opened (all under `eval/pilots/red-team-2026-11-02/bundles/B5/`): `RESEARCH_STATE.md`, `EXPERIMENT_LEDGER.md`, `HYPOTHESES.md`, `PROTOCOL.md`. No other file under `eval/` was opened. Shell use: `cat -n`, `ls`, `test -d`, one `python3 -c` recomputation from ledger rows (no scipy on the host; the t-distribution CDF was implemented via the regularized incomplete beta and sanity-checked: p(t = 3.182, df = 3) = 0.0500).

## 1. Verdict

PROCEED-WITH-CHANGES

The deciding premise is C1's significance statement: the per-seed rows in the ledger give Welch t = 1.16, df = 3.12, two-sided p = 0.33, not p = 0.04, so H_001 is not confirmed under its own pre-registered rule; and C3 asserts an ablation result whose only run (RUN_010) is still `planned`. Both are fixed by text changes that spend no compute and keep the question, arms and primary comparison.

Named changes (the lock proceeds after them):
1. C1 → "H_001 not confirmed under the pre-registered rule: +0.013 nDCG@10 (95 % CI −0.021 to +0.047), Welch two-sided p = 0.33, df ≈ 3.1, n = 3 per arm; the margin (0.010) is met, the significance condition is not." Update `HYPOTHESES.md:20` to match (the rule's own word is "refuted otherwise").
2. C3 → remove from the lock or mark "planned (H_003, RUN_010, launch 2026-11-05); no claim". If the paper needs the ablation, wait for RUN_010 (≈ 30 GPU-h, fits before 2026-11-19) and lock C3 from its rows.
3. Lock C2 and C4 as written; they reproduce.

## 2. Premise audit

### P1 — C1 is significant at α = 0.05 (p = 0.04). Status: FALSE (load-bearing)

Quoted: `RESEARCH_STATE.md:40-41` "The difference, +0.013, meets the pre-registered margin (0.010) and the Welch t-test is significant at α = 0.05 (p = 0.04, df ≈ 3.1)." Same number at `HYPOTHESES.md:20` "outcome: confirmed (+0.013, p = 0.04; RESEARCH_STATE C1)".

Rule: `HYPOTHESES.md:17` / `PROTOCOL.md:38-39` "confirmed iff mean difference >= 0.010 and Welch p < 0.05 (two-sided); refuted otherwise". Test definition `PROTOCOL.md:47-48`: "Welch two-sided t-test on per-seed test metric (n = 3 per arm), α = 0.05".

Evidence (recomputed from `EXPERIMENT_LEDGER.md:26-28` in-batch test nDCG@10 0.503 / 0.509 / 0.491 and `:30-32` Kestrel 0.512 / 0.498 / 0.531):

```
in-batch  mean=0.5010 sd=0.0092
Kestrel   mean=0.5137 sd=0.0166
diff=+0.0127  se=0.0109  Welch t=1.159  df=3.12  two-sided p=0.327
95% CI for diff: (-0.0214, +0.0467); t_crit(df=3.12)=3.114; min detectable diff at alpha=.05 = 0.0340
```

Assumed p = 0.04; found p = 0.33. The claimed df ≈ 3.1 reproduces (3.12), so the test family is the one the protocol names; only the p-value is wrong. No obvious variant reaches 0.04 either: Student pooled (df = 4) p = 0.311; Welch one-sided p = 0.164; paired-by-seed (df = 2) p = 0.483; dev-split Welch p = 0.313. Under the rule as written, H_001 is "refuted" (margin met, p ≥ 0.05); the honest reading is "not confirmed at n = 3". The 95 % interval contains zero and contains negative effects larger than the margin.

### P2 — C3's ablation result exists. Status: FALSE (load-bearing)

Quoted: `RESEARCH_STATE.md:44-47` "**C3 (H_003).** The gain is attributable to the mined hard negatives: training the same model with uniformly sampled negatives in place of the BM25-mined ones removes it, so the mining step, not the larger negative cache, is the active ingredient."

Evidence: `EXPERIMENT_LEDGER.md:34` RUN_010 status `planned`, all four metric cells `—`, notes "launch 2026-11-05"; `HYPOTHESES.md:35-36` linked_runs `[RUN_010]`, outcome `""` "(not launched; planned 2026-11-05)"; `RESEARCH_STATE.md:21` "negatives-ablation: RUN_010 planned (H_003)" and `:29` "experiment-runner: launch RUN_010 on 2026-11-05". Assumed: an observed removal of the gain under uniform negatives; found: no run has started and no metric exists. C3 is written in the indicative with no status tag, where C1 says "(H_001, confirmed)" and C2 "(H_002, inconclusive)" (`RESEARCH_STATE.md:37`, `:42`, `:45`).

### P3 — What the measured quantity really is. Status: VERIFIED (at the level of the four files)

The number in C1 is test nDCG@10 with graded relevance 0–2 (`EXPERIMENT_LEDGER.md:51-52`, `PROTOCOL.md:34`), on 1,000 test queries disjoint by source document from train/dev (`PROTOCOL.md:6-7`), scored once per run at the epoch with the highest dev nDCG@10 against the full 1.2 M-passage index by `scripts/eval_test.py` (`PROTOCOL.md:8-10`, `:28-29`; `EXPERIMENT_LEDGER.md:17-19`). Every learned row carries `--select dev` and `ckpt` = "best dev" (`EXPERIMENT_LEDGER.md:26-28`, `:30-33`). Both learned arms share the 6-layer encoder, lr 2e-5, warm-up 5 %, batch 128, 20 epochs (`EXPERIMENT_LEDGER.md:16-17`, `PROTOCOL.md:15-20`); Kestrel adds 7 hard negatives per query from a cache of 16 mined from BM25 top-100, re-mined every 2 epochs (`PROTOCOL.md:17-19`). Seeds {0, 1, 2} fixed before launch (`PROTOCOL.md:23`).

Caveats that keep this at "file level": `cfg/inbatch.yaml`, `cfg/kestrel.yaml` and `scripts/eval_test.py` are not in the bundle, so "same model except the negatives" rests on ledger and protocol prose. "Budget identical" (`PROTOCOL.md:20`) means identical lr / epochs / batch, not identical FLOPs — the Kestrel arm scores 7 extra passages per query per step.

### P4 — The aggregates in C1, C2, C4 and the summary table reproduce from the rows. Status: VERIFIED

Recomputed from `EXPERIMENT_LEDGER.md:26-28` and `:30-32` (sample sd, n = 3), compared with `RESEARCH_STATE.md:38-50`, `:58-59` and `EXPERIMENT_LEDGER.md:41-42`:

| metric | in-batch (recomputed) | Kestrel (recomputed) | diff | matches claim |
| --- | --- | --- | --- | --- |
| test nDCG@10 | 0.5010 ± 0.0092 → 0.501 ± 0.009 | 0.5137 ± 0.0166 → 0.514 ± 0.017 | +0.0127 | yes |
| test R@50 | 0.7240 ± 0.0128 → 0.724 ± 0.013 | 0.7310 ± 0.0131 → 0.731 ± 0.013 | +0.0070 | yes |
| test MRR@10 | 0.4767 ± 0.0091 → 0.477 ± 0.009 | 0.4897 ± 0.0160 → 0.490 ± 0.016 | +0.0130 | yes |

Margin checks: nDCG +0.01267 ≥ 0.010 → margin met (`RESEARCH_STATE.md:40` correct). R@50 +0.0070: not ≥ 0.010 and not < 0 → "inconclusive" under `HYPOTHESES.md:25` (`RESEARCH_STATE.md:42-44` and `HYPOTHESES.md:28` correct; Welch p = 0.54 for the record). BM25 row values (0.412 / 0.655 / 0.389) copied correctly from `EXPERIMENT_LEDGER.md:25` to `RESEARCH_STATE.md:57`.

### P5 — `scripts/report_p1.py` at `e13f7a9` is the source of record and reproduces the claim numbers. Status: UNVERIFIED

Quoted: `RESEARCH_STATE.md:10`, `:24` "claim numbers frozen 2026-10-29 from scripts/report_p1.py at e13f7a9", `:61`; `PROTOCOL.md:49-50`. The script is not in the bundle and no run output of it is quoted anywhere. The check that settles it: run `scripts/report_p1.py` at code_rev `e13f7a9` on the ledger rows and diff its output against the aggregates table and C1's p-value. If it prints p = 0.04, read what test it runs — it cannot be the per-seed Welch test of `PROTOCOL.md:47-48` (which yields p = 0.33 and the df ≈ 3.1 the claim itself cites). Whichever way that goes, the claim as written does not follow from the ledger rows.

### Other FALSE premises

- `HYPOTHESES.md:20` "outcome: confirmed (+0.013, p = 0.04; RESEARCH_STATE C1)" — the same wrong p-value, recorded in a second file; under `HYPOTHESES.md:17` the outcome is not "confirmed".

## 3. Strongest alternative

Three competitors to "lock as written":

(a) Do nothing now — hold the whole lock until RUN_010 reads out. RUN_010 launches 2026-11-05 at ≈ 30 GPU-h (`HYPOTHESES.md:34`, `EXPERIMENT_LEDGER.md:34`), so a lock around 2026-11-08 still leaves eleven days before the 2026-11-19 deadline (`RESEARCH_STATE.md:26`). Cost: the paper-writer's draft (`RESEARCH_STATE.md:28`) waits for nothing it needs, since C1, C2 and C4 do not change with RUN_010.

(b) The cheaper test — for C1 there is none to run: the correction is one recomputation from rows already in the ledger, done above at zero compute. For C3 the cheapest honest version is to drop it from the lock and state "ablation pending (RUN_010)".

(c) Lock a corrected set now: C1 reworded as the not-confirmed estimate with its interval and p; H_001's outcome line updated; C3 removed or marked planned; C2 and C4 unchanged. Zero compute; a few lines of text.

(c) dominates for C1/C2/C4 and (a) dominates for C3 alone — i.e. lock the corrected C1/C2/C4 today so drafting can start, and keep C3 open until RUN_010's rows exist. Before spending the 30 GPU-h, note that with sd ≈ 0.009–0.017 and n = 3 the minimal detectable difference under the H_003 rule (margin 0.010, p < 0.05) is ≈ 0.034: the ablation can only "confirm" if uniform negatives cost ≈ 0.03 nDCG or more. The operator may want to register that power limitation alongside H_003 rather than discover it on 2026-11-08.

## 4. Kill criteria

- For the lock as written: the observable result that stops it is already in hand — the per-seed Welch test on `EXPERIMENT_LEDGER.md:26-28` vs `:30-32` gives p = 0.33 (≥ 0.05). Cost: one `python3 -c` on six numbers, zero compute.
- For the corrected lock: stop if `scripts/report_p1.py` at `e13f7a9` produces aggregates that differ from the ledger rows (that would mean the rows, not only the claim, are stale and the whole table must be re-derived). Cost: one script run, minutes.
- For C3 if deferred to RUN_010: stop the ablation claim if the 3-seed mean test nDCG@10 of `cfg/kestrel_randneg.yaml` is within 0.010 of 0.514, or if the Welch p ≥ 0.05 — and expect the latter unless the drop is ≈ 0.03 or larger (see §3). Cost: the already-planned ≈ 30 GPU-h.

## 5. Regret list

1. Not re-running the Welch test from the six per-seed values before writing "p = 0.04" into two files. The df reproduces and the p does not, which points to a transcription error or a different test than the protocol names; either way a reviewer with the ledger will find it in one line of arithmetic.
2. Not showing the per-seed picture next to the mean: Kestrel's +0.013 is carried by seed 2 (0.531 vs 0.491); seeds 0–1 alone give 0.505 vs 0.506. The seed-1 Kestrel run (0.498) is below two of three in-batch runs.
3. Not computing the minimal detectable difference (≈ 0.034 at n = 3, α = 0.05) before pre-registering a 0.010 margin with p < 0.05. As written, the H_001 rule could essentially never confirm a true 0.010 effect; H_003 (`HYPOTHESES.md:33`) repeats the same rule at the same n.
4. Locking C3 while RUN_010 is `planned`. If the ablation refutes, the locked text has to be withdrawn after submission; if it confirms, nothing was gained by locking early.
5. Citing `scripts/report_p1.py` as the source of record six times without one line of its output in any of the four files.

## 6. What is solid

- Every mean ± sd in `RESEARCH_STATE.md:55-59` and `EXPERIMENT_LEDGER.md:40-42` reproduces from the rows at three decimals (table in P4).
- The nDCG@10 difference +0.0127 meets the 0.010 margin; C2's "inconclusive" is correct under the H_002 rule (+0.007: not ≥ 0.010, not < 0).
- MRR@10 in C4 (0.490 ± 0.016 vs 0.477 ± 0.009) reproduces; no claim is made on it.
- Checkpoint selection reads dev only and test is scored once at the selected checkpoint: `--select dev` on every learned row (`EXPERIMENT_LEDGER.md:26-33`), `PROTOCOL.md:8-10`, `:28-29`.
- Pre-registration order holds: H_001 and H_002 dated 2026-09-23 (`HYPOTHESES.md:15`, `:23`), P1 frozen 2026-09-27 (`PROTOCOL.md:1`), first run 2026-10-01 (`EXPERIMENT_LEDGER.md:25`). H_003 dated 2026-10-20 (`HYPOTHESES.md:31`) precedes RUN_010's planned launch.
- RUN_005's abort is handled as `PROTOCOL.md:30-31` requires: new row RUN_006 with identical config, aborted row kept with its reason (`EXPERIMENT_LEDGER.md:29-30`, `:46-47`; `RESEARCH_STATE.md:66-68`).
- RUN_009 is `pre_reg: false`, single seed, in no claim (`EXPERIMENT_LEDGER.md:33`, `HYPOTHESES.md:39-44`), and C4 excludes it (`RESEARCH_STATE.md:51`).
- BM25 floor parameters k1 = 0.9, b = 0.4 agree across `EXPERIMENT_LEDGER.md:25`, `PROTOCOL.md:13`, `RESEARCH_STATE.md:49`; it is excluded as a comparison target in all three.
- n = 3 is stated as the budget and in captions (`RESEARCH_STATE.md:32`, `:53`, `:65`; `PROTOCOL.md:23-24`).
- The timeline for the deferred C3 is feasible: RUN_010 at ≈ 30 GPU-h from 2026-11-05 finishes well before 2026-11-19.

## 7. Minor

- C3 carries no status tag where C1 and C2 do (`RESEARCH_STATE.md:37`, `:42`, `:45`); whatever the operator decides, give it one.
- `RESEARCH_STATE.md:72` says RUN_009 "appears only in the ledger and H_004", while `:51` and `:71` mention it themselves; harmless.
- `HYPOTHESES.md:19` lists RUN_001 (BM25) among H_001's linked runs although `PROTOCOL.md:13-14` excludes BM25 as a comparison target; harmless, but it invites the question.
