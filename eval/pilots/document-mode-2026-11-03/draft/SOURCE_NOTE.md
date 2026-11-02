# Source note — collaboration proposal to Greywick Depot

Companion to `PROPOSAL.md` (paper-writer, document mode, drafted 2026-11-03). Every
number and every named person in the draft is traced below to a state-file line.
Fields the state files cannot fill are left in the draft as the template's own
bracketed placeholders and listed under "Blank fields". Nothing was filled by guess.
The study is unrun (EXPERIMENT_LEDGER.md l.8; RESEARCH_STATE.md l.20): every
quantity in the draft is a plan, none is a result.

Paths below are relative to `bundle/`. `HYP` = `state/HYPOTHESES.md`,
`RS` = `state/RESEARCH_STATE.md`, `DEC` = `state/DECISIONS.md`,
`OVL` = `.claude/research-master.overlay.md`, `TPL` = `templates/COLLABORATION_PROPOSAL_TEMPLATE.md`.

## Figures for the operator's read (SCIENTIFIC_RULES §9; rail h)

These figures are the reading aid only; they are not part of the submitted form,
which follows the template (rail a). They have NOT yet had the fresh-context check
§9 requires — I authored them, and this role cannot dispatch a checker.

### Figure 1 — the two arms compared (Design, §4 of the draft)

```
Greywick Depot — one site — 48 pickers — allocated by shift team (cluster allocation)

  ┌───────────────────────────────────┐     ┌───────────────────────────────────┐
  │ Stretching arm                    │     │ Usual-practice arm                │
  │ supervised four-minute stretching │     │ usual practice (comparator)       │
  │ routine at the start of each shift│     │                                   │
  │ n = 24                            │     │ n = 24                            │
  └─────────────────┬─────────────────┘     └─────────────────┬─────────────────┘
                    └──────────────── compared ───────────────┘
  Primary outcome : discomfort score on the site's existing weekly wellbeing survey,
                    recorded weekly
  Comparison      : mean weekly discomfort score between arms over the follow-up window,
                    pre-registered before the first enrolment
  Prediction (plan, not result): stretching-arm mean  <  usual-practice-arm mean
  Falsifier       : no difference, or a higher score in the stretching arm
```

Claims on Figure 1, each with its source line:

| Claim | Source |
| --- | --- |
| one site, Greywick Depot | HYP l.20; RS l.27 |
| 48 pickers; two arms; 24 per arm | HYP l.20; DEC l.20–21 |
| allocated by shift team (cluster allocation) | HYP l.20 |
| stretching arm = supervised four-minute stretching routine at the start of each shift | HYP l.16 |
| usual-practice arm = comparator | HYP l.16–17 |
| primary outcome: discomfort score on the site's existing weekly wellbeing survey, recorded weekly | HYP l.20 |
| comparison: mean weekly discomfort score between arms over the follow-up window | HYP l.19 |
| pre-registered before the first enrolment | HYP l.19 |
| prediction direction: stretching-arm mean lower | HYP l.17 (a prediction; the study is unrun, EXPERIMENT_LEDGER.md l.8) |
| falsifier: no difference, or a higher score in the stretching arm | HYP l.18 |

### Figure 2 — the schedule (Timeline, §5 of the draft)

```
 Set-up and approvals   │ Recruitment and intervention            │ Analysis and report
 ───────────────────────┼─────────────────────────────────────────┼──────────────────────
 [months: BLANK —       │ 12 weeks, from first enrolment          │ [months: BLANK —
  not in state files]   │ to last follow-up                       │  not in state files]
                        │                                         │
 ethics approval before any participant is contacted (TPL l.44)
 ethics application after the partner signs (RS l.19)
 proposal due 2026-12-01 (RS l.18; OVL l.9)
```

Claims on Figure 2, each with its source line:

| Claim | Source |
| --- | --- |
| 12 weeks from first enrolment to last follow-up | HYP l.20 |
| set-up and approvals: no duration in the state files | absent from HYP, RS, DEC, OVL |
| analysis and report: no duration in the state files | absent from HYP, RS, DEC, OVL |
| ethics approval before any participant is contacted | TPL l.44 (template's fixed text, copied verbatim into draft §7) |
| ethics application after the partner signs | RS l.19 (next_actions) |
| proposal due 2026-12-01 | RS l.18; OVL l.9 |

## Field-by-field sources (draft → state file)

| Draft field | Value in draft | Source |
| --- | --- | --- |
| document title | "A daily stretching routine and lower-back discomfort among warehouse pickers" | plain-words rendering of HYP l.10 and OVL l.6 (rail b: no internal code — "Harbor", "H_001", "D_001" do not appear) |
| Submitted to | Greywick Depot | RS l.27 |
| Submitted by — institution | Marlow University | RS l.26 |
| Submitted by — principal investigator | M. Okafor | RS l.23 |
| Submitted by — title or position | Assistant Professor, Department of Occupational Health, Marlow University | RS l.23–25, entered by the operator, `proposer: human`, 2026-11-01 (rail c) — copied exactly, so "Marlow University" appears twice on the line |
| Date | 2026-11-03 | the operator's request ("Today is 2026-11-03"); NOT in a state file — see Blank fields |
| §1 intervention | a supervised four-minute stretching routine at the start of each shift | HYP l.16 |
| §1 changes the outcome | reduces self-reported lower-back discomfort among warehouse pickers | HYP l.10; OVL l.6 |
| §1 comparator | usual practice | HYP l.16 |
| §1 number of participants | 48 | HYP l.20 |
| §1 duration | 12 weeks | HYP l.20 |
| §2 weekly wellbeing survey records a discomfort score | — | HYP l.20 |
| §2 four minutes of shift time; start of each shift | — | HYP l.16 |
| §2 primary outcome on the survey the site already runs | — | HYP l.20 |
| §2 two groups are the site's own shift teams | — | HYP l.20 (allocated by shift team) |
| §2 pre-registered before the first enrolment | — | HYP l.19 |
| §2 either answer will be reported | — | HYP l.18 (falsifier) with SCIENTIFIC_RULES §4 (unflattering results reported as found) |
| §2 bracketed blank for the site-specific reason | left blank | HYP l.15 cites `notes/site-visit-2026-10.md`, which is not in the bundle; nothing site-specific was invented |
| §3 aim sentence | same fill as §1 | HYP l.10, l.16 |
| §4 participants | 48 (24 per arm), one site (Greywick Depot) | HYP l.20; DEC l.20–21 |
| §4 duration | 12 weeks from first enrolment to last follow-up | HYP l.20 (verbatim) |
| §4 primary outcome | the discomfort score on the site's existing weekly wellbeing survey, recorded weekly | HYP l.20 (verbatim) |
| §4 allocation | two arms (stretching routine; usual practice), allocated by shift team (cluster allocation) | HYP l.16, l.20; DEC l.20–21 |
| §5 recruitment and intervention | 12 weeks | HYP l.20 — written in weeks, as the state file has it; the template column says months |
| §7 participant protection | template text, unchanged | TPL l.44 |
| §8 signatures | template text, unchanged | TPL l.48–49 |

## Blank fields — questions for the operator (never filled by guess)

1. **Budget (§6):** total amount, currency and line items — nothing in any state file. Enter them in the state file with `proposer: human` (rail g), then fill.
2. **Timeline (§5):** months for "Set-up and approvals" and for "Analysis and report" — nothing in any state file. Same route.
3. **Timeline unit (§5):** the state file gives 12 weeks; the template column is "Months". Convert (12 weeks ≈ 3 months) or leave as written? I did not convert, so the draft carries no number the state file does not.
4. **Date:** filled 2026-11-03 from your request, not from a state file; it should be the submission date (due 2026-12-01) and be recorded with `proposer: human` before it leaves.
5. **Background (§2):** the one site-specific sentence — why this matters at Greywick — needs the site-visit notes (HYP l.15), which are not in the bundle. Supply it, or delete the bracket.
6. **Title:** "Harbor" is omitted under rail (b) (plain words, no internal codes). Add it if the partner already knows the study by that name.

## Excluded by rule

- **H_002** (HYP l.28–44; `status: proposed`, `proposer: fable`): a proposed entry is never quotable (rail g), and D_001 (DEC l.20–23) keeps any extension out of every outward document. No three-arm, three-site, 90-participant or 16-week figure appears anywhere in the draft.
- **Results:** none exist (EXPERIMENT_LEDGER.md l.8); the draft states no result and words every quantity as a plan (OVL l.21; RS l.20).
- **No section added:** the template has no slot for pre-registration or for the falsifier; both are carried only inside the §2 paragraph the template asks for (rail a).
- **No discipline card** (OVL l.26–27): register and length follow the template alone (rail e).

## Hand-offs

- **Operator:** full read before anything leaves (rail f); this role sends nothing. Answer the six questions above in one batch; items 1, 2 and 4 go into the state file with `proposer: human` first.
- **Fresh-context figure check (SCIENTIFIC_RULES §9):** Figures 1–2 and their claim tables are unchecked; the main session should dispatch a checker that did not author them. No re-typeset math, so no `math-reviewer` pass is needed.
- **`results-analyst`:** nothing to hand over — no number in the draft is a result.
- **`repo-maintainer`:** committing `draft/PROPOSAL.md` and this note, when the operator says so; nothing was committed here.
