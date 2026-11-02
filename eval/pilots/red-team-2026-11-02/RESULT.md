# Result — red-team pilot (DECISION_010 condition), 2026-11-02

**Verdict, by the decision rule of `PREREG.md` applied literally: `mode`.**
Arm B (the red-team draft contract) found exactly as many planted threats as arm A
(`sparring-partner`): 7 of 7 each, every one graded full. On the two sound controls
arm B made 7 false positives; arm A made 0. The rule adds the role only if B finds
strictly more **and** B's false positives are not greater than A's; neither holds.
The roster stays at fourteen. The two residual triggers ("after a surprising
result", "on request") are documented as a dispatch mode of `sparring-partner`
(DECISION_010, `proposed`).

## 1. Unblinded grades (`reports/GRADES.md` joined with the sealed map)

Threatened bundles: B1 (2 planted threats), B2 (3), B5 (2). Sound controls: B3, B4.

| label | raw report | arm | bundle | threatened | threats full / partial / missed | false positives (controls) or extra (threatened) | refuting tests named / threats found |
| --- | --- | --- | --- | --- | --- | --- | --- |
| R10 | A_B1.md | A | B1 | yes | 2 / 0 / 0 (B1-T1, B1-T2) | extra 0 | 2 / 2 |
| R03 | B_B1.md | B | B1 | yes | 2 / 0 / 0 (B1-T1, B1-T2) | extra 4 (A3, A4, A5, A7; all minor) | 2 / 2 |
| R06 | A_B2.md | A | B2 | yes | 3 / 0 / 0 (B2-T1, B2-T2, B2-T3) | extra 0 | 3 / 3 |
| R05 | B_B2.md | B | B2 | yes | 3 / 0 / 0 (B2-T1, B2-T2, B2-T3) | extra 0 | 3 / 3 |
| R02 | A_B3.md | A | B3 | no | n/a | 0 | n/a |
| R07 | B_B3.md | B | B3 | no | n/a | 4 (A1, A2, A3 not in bundle, ranked load-bearing; A4 = B3-L7, minor) | n/a |
| R01 | A_B4.md | A | B4 | no | n/a | 0 | n/a |
| R04 | B_B4.md | B | B4 | no | n/a | 3 (A1 not in bundle; A2 = B4-L9; A3 not in bundle; all ranked load-bearing) | n/a |
| R08 | A_B5.md | A | B5 | yes | 2 / 0 / 0 (B5-T1, B5-T2) | extra 0 | 2 / 2 |
| R09 | B_B5.md | B | B5 | yes | 2 / 0 / 0 (B5-T1, B5-T2) | extra 3 (A5 key-named extra; A6 = B5-L4; A7 = B5-L8; all minor) | 2 / 2 |

## 2. Sums over the three threatened bundles (7 planted threats)

| arm | found = full + ½·partial | missed | false positives on the controls (B3 + B4) | extra on threatened bundles | refuting test named / threats found |
| --- | --- | --- | --- | --- | --- |
| A — `sparring-partner` | 7 + ½·0 = **7** | 0 | **0** | 0 | 7 / 7 |
| B — red-team draft | 7 + ½·0 = **7** | 0 | **7** | 7 | 7 / 7 |

## 3. The pre-registered rule, applied

> Add the `red-team` role (accept DECISION_010 as "add") only if arm B finds
> strictly more planted threats (full + ½ partial) than arm A **and** B's false
> positives on the controls are not greater than A's. A tie, or A ≥ B, keeps the
> roster at fourteen; the two residual triggers are then documented as a
> dispatch mode of `sparring-partner`.

- Condition 1, B strictly more found: 7 > 7 — **false**.
- Condition 2, B's false positives ≤ A's: 7 ≤ 0 — **false**.
- Verdict: **`mode`**.

**Sensitivity.** The grader flagged four construction artefacts of the synthetic
bundles that the key classifies neither as threats nor as honest limitations
(`GRADES.md`, grader note 1a–1d) and scored them against the key as written.
Reclassifying all of them as legitimate findings would lower arm B's false
positives from 7 to 5 (R07 A1, R04 A3) and its extras from 7 to 5; no
planted-threat grade depends on them. The verdict does not change: condition 1
fails on its own.

## 4. Secondary observations (reported, not decisive)

- **Refuting tests.** Both arms named a concrete, runnable check for every
  threat they found (7 / 7 each).
- **Where arm B's false positives came from.** On both controls the draft
  contract ranked as load-bearing "holes" things the bundle does not contain or
  lists as an honest limitation (3 on B4, 4 on B3). Where arm A noticed the same
  things it filed them as UNVERIFIED premises or regret-list items, which the
  rubric does not count. The attack framing, not the detection, is what differed.
- **Cost for the same detection** (subagent token usage and wall time reported
  by the harness; report length from `wc -l`):

| arm | tokens per dispatch (B1…B5) | mean tokens | wall time (s) | report lines (B1…B5) | mean lines |
| --- | --- | --- | --- | --- | --- |
| A | 55 486 · 50 876 · 47 959 · 60 096 · 52 920 | 53 467 | 327 · 250 · 236 · 344 · 276 | 86 · 86 · 70 · 78 · 113 | 87 |
| B | 78 535 · 82 678 · 83 048 · 93 620 · 85 048 | 84 586 | 304 · 321 · 373 · 471 · 395 | 315 · 278 · 298 · 321 · 288 | 300 |

  Arm B used about 1.6× the tokens and wrote about 3.5× the text of arm A.
  Material: designer 191 803 tokens (1 294 s); grader 206 417 tokens (723 s).

## 5. Protocol record and deviations

- **Dispatch.** Ten agents in one parallel batch (open profile). Same model
  (`inherit`) and same effort for both arms: `xhigh` via the `sparring-partner`
  wrapper (`core/effort_policy.yaml`) for A, the session effort `xhigh` for B
  (no `model` or `effort` parameter). Prompts were one template per arm,
  differing only in the bundle number and the output path. Both arms received
  the same read-only shell line, the same "recompute from the ledger rows" line
  and the same forbidden-reads list; the recompute line is arm A's overlay slot
  in `DISPATCH_BRIEFS.md` §1 and was given to arm B as well so the inputs stayed
  equal (orchestrator's call, recorded here). All ten returned
  `ACK RULES_HASH=0f0b1f5cddf7` as the first line of the report file and of the
  hand-back; none failed.
- **Blinding.** Script of `DISPATCH_BRIEFS.md` §2; map sealed at mode 600,
  git-ignored. Self-naming words (`sparring`, `red-team`, `black-faced`) were
  redacted to `<role>` in the blind copies (0 hits after). Limitation, known at
  pre-registration: the two contracts' output shapes differ (verdict + premise
  audit vs `HOLES` + ranked attacks) and the reports differ about 3.5× in
  length, so a grader could infer the contract from structure. Only the names
  were blinded; the grader was told not to try.
- **Deviation 1 — material written twice.** The designer agent dispatched in
  the previous session survived that session's `/clear` and finished its
  bundles and key after a second designer had been dispatched from this session
  with the same specification. The first designer's hand-back named the
  threatened bundles and their threat counts (B1, B3, B4), breaking the
  orchestrator's blind to the assignment although not to the threats. Remedy:
  the second designer was told to delete that material and write its own from a
  fresh random draw; its hand-back revealed nothing, and the final key's
  assignment (B1, B2, B5) differs from the discarded one. The arm prompts were
  identical templates in any case, so no per-bundle knowledge could reach them.
- **Deviation 2 — orchestrator exposure to verdict lines.** Per the brief's
  return format, each arm's verdict or `HOLES` line (one sentence summarising
  its worst finding) reached the orchestrator before grading. The orchestrator's
  remaining duties were mechanical (blinding script, grader dispatch, join,
  rule) and the grader never saw those lines. Recorded; no effect on grades.
- **Forbidden-reads gap.** Two reports (R03, R09) opened
  `templates/project-state/OPEN_QUESTIONS.md`, outside `eval/`, for an entry
  schema. The arm prompts restricted reads under `eval/` only; the grader found
  no sign that any report saw the key, the map, the pre-registration's threat
  list or another bundle.
- **Stopping rule.** One run of ten dispatches; no re-dispatch; nothing
  deleted. The key and the map stay sealed (`eval/.sealed/`, git-ignored, mode
  600). The bundles, the raw and blind reports, `GRADES.md` and this file are
  committed as the pilot's record.

## 6. Files

- `PREREG.md` — the pre-registration (binding).
- `RED_TEAM_DRAFT.md` — the candidate contract (pilot only; not a role).
- `DISPATCH_BRIEFS.md` — the prompts, the blinding script, the grading brief.
- `bundles/B1..B5/` — the five synthetic mini-projects (20 files).
- `reports/raw/{A,B}_B{1..5}.md` — the ten reports as written.
- `reports/blind/R01..R10.md` — the blinded, redacted copies the grader read.
- `reports/GRADES.md` — the grader's table, per-report detail and notes.
- `eval/.sealed/red-team-pilot-key.md`, `eval/.sealed/red-team-pilot-map.md` — sealed, never committed.
