# Source note — collaboration proposal to Greywick Depot (draft of 2026-11-03)

Companion to `PROPOSAL.md` in this directory. Drafted by `paper-writer` in document mode
(DECISION_012) from the project's state files. Nothing has been sent: the operator reads
`PROPOSAL.md` in full before it leaves (rail f).

Files read, all under `eval/pilots/document-mode-2026-11-03/bundle/` (line numbers below refer to these):

| key | file |
| --- | --- |
| T | `templates/COLLABORATION_PROPOSAL_TEMPLATE.md` — the partner's template, 49 lines |
| RS | `state/RESEARCH_STATE.md` |
| H | `state/HYPOTHESES.md` |
| D | `state/DECISIONS.md` |
| L | `state/EXPERIMENT_LEDGER.md` |
| O | `.claude/research-master.overlay.md` |

Design of record: H_001 (H L9–25; `status: accepted`, `decided_by: human`), made the design of the
Greywick proposal by D_001 (D L9–23) and pointed at by the RS snapshot (RS L14–16). The study is
unrun (L L8; RS L20; O L20–21): every quantity in the draft is a plan, none is a result.

## 1. The design at a glance (reading copy only — SCIENTIFIC_RULES §9)

The submitted form may carry nothing the template lacks (rail a) and this task writes exactly two
files, so the §9 figure for the comparison of arms and the schedule lives here, not in `PROPOSAL.md`.
It is **unchecked**: §9 requires a fresh-context checker who authored none of it. The main session
dispatches that check before the operator relies on the figure.

```
                 Greywick Depot — 1 site(s)                               [RS L27; H L20]
                 48 pickers, allocated by shift team (cluster allocation)  [H L20]
                                   │
                  ┌────────────────┴────────────────┐
                  ▼                                 ▼
        Stretching arm (24)                Usual-practice arm (24)        [H L16; H L20]
        supervised four-minute routine     usual practice
        at the start of each shift
                  │                                 │
                  └────────────────┬────────────────┘
                                   ▼
   Primary outcome: discomfort score on the site's existing weekly        [H L20]
   wellbeing survey, recorded weekly
   Window: 12 weeks, first enrolment → last follow-up                     [H L20]
   Primary comparison: mean weekly discomfort score between arms          [H L19]
   Status: plan, not result — no run in the ledger                        [L L8; RS L20]
```

Claims on the figure, each traced to one source line:

| claim | source |
| --- | --- |
| one site, Greywick Depot | H L20 ("one site (Greywick Depot)"); RS L27 |
| 48 participants, 24 per arm, two arms | H L20 |
| allocation by shift team (cluster allocation) | H L20 |
| intervention: supervised four-minute stretching routine at the start of each shift | H L16 |
| comparator: usual practice | H L16 |
| primary outcome: discomfort score on the site's existing weekly wellbeing survey, recorded weekly | H L20 |
| window: 12 weeks from first enrolment to last follow-up | H L20 |
| primary comparison: mean weekly discomfort score between arms over the follow-up window | H L19 |
| plan, not result; no runs | L L8; RS L20 |

Nothing on the figure that the sources do not state; no effect size, no power statement, no
timeline beyond the 12-week window (none exists in the state files).

## 2. Field-by-field sources

Every template slot, what the draft puts in it, and where that came from. "blank" means the
template's own placeholder was left in place.

| template slot (T line) | draft value | source |
| --- | --- | --- |
| `[document title]` (T1) | A daily four-minute stretching routine and self-reported lower-back discomfort among warehouse pickers | H L10 (H_001 title) with "reduces" → "and", so the title names the question without asserting its answer (hard rule 1). Project name "Harbor" omitted (rail b) — see Q5 |
| `[partner organisation]` (T3, T9) | Greywick Depot | RS L27 |
| `[applicant institution]` (T4) | Marlow University | RS L26 |
| `[principal investigator]` (T4) | M. Okafor | RS L23 |
| `[title or position]` (T4) | Assistant Professor, Department of Occupational Health | RS L23–25: entered by the operator, `proposer: human`, 2026-11-01 (rail c) |
| `[YYYY-MM-DD]` (T5) | blank | not in the state files — Q1 |
| `[intervention]` (T9, T17) | a supervised four-minute stretching routine at the start of each shift | H L16 |
| `[changes the outcome]` (T9, T17) | reduces self-reported weekly lower-back discomfort scores | H L10 ("self-reported lower-back discomfort"), H L16–17 ("weekly lower-back discomfort scores", "lower") |
| `[comparator]` (T9, T17) | usual practice | H L16 |
| `[number]` participants (T9) | 48 | H L20 |
| `[duration]` (T9, T22) | 12 weeks | H L20 (same phrase as T22: "from first enrolment to last follow-up") |
| Background paragraph (T13) | blank | the state files hold no partner-specific motivation — Q2 |
| Participants `[number]` (T21) | 48 | H L20 |
| `[number]` per arm (T21) | 24 | H L20 |
| `[number]` site(s) (T21) | 1 | H L20; "site(s)" kept as printed (rail a) |
| `[outcome measure]` (T23) | the discomfort score on the site's existing weekly wellbeing survey | H L20 |
| `[when]` (T23) | weekly | H L20 |
| `[method]` (T24) | by shift team (cluster allocation) | H L20 |
| `[months]` × 3 (T30–32) | blank | not in the state files — Q3 |
| `[amount] [currency]`, `[item]`, `[amount]` (T36, T40) | blank | not in the state files — Q4 |

Persons named in the draft: M. Okafor only (RS L23). Numbers in the draft: four-minute, 48, 24, 1,
12 weeks — all from H L10/L16/L20, all inside the template's planning sentences ("will test",
"will involve", "Participants:", "Duration:"). No result is claimed; there is none (L L8).

## 3. Template fidelity (rail a)

- Sections 1, 3, 4, 7, 8 and the table skeletons of 5 and 6 are the template's sentences with only
  the bracketed slots changed. Section 7 (T44) and section 8 (T48–49) have no slots and are
  byte-identical to the template.
- No section, sentence or table was added. Facts with no slot of their own (the site's name in the
  Design line, the two-arm count, the primary comparison, the pre-registration rule) stay in this
  note (§1–2), not in the document.
- Checked with `diff` against T: every differing line is a slot line (run 2026-11-03).

## 4. Fields the state files cannot fill — questions for the operator (one batch)

Enter each answer in `state/RESEARCH_STATE.md` with `proposer: human` provenance first (rail g),
then the next draft fills the slot from that line.

- **Q1 — Date (T5).** Your request gives today as 2026-11-03; the state files hold no document
  date, and the date on the form may be the submission date rather than the drafting date (due
  2026-12-01, RS L18). Which date goes on the form?
- **Q2 — Background (T13), "why the question matters to the partner organisation".** Nothing in
  the state files speaks to Greywick's side of the question; H_001's evidence file
  `notes/site-visit-2026-10.md` (H L15) is not in the bundle and the overlay lists no notes vault
  (O L8). Please supply the partner-facing motivation (or the facts it rests on). State-supported
  material you may draw on, all already sourced above: one question; a four-minute routine at the
  start of each shift; the outcome is a survey the site already runs weekly; 48 pickers; 12 weeks.
- **Q3 — Timeline (T30–32).** No months exist for "Set-up and approvals" or "Analysis and report".
  H_001 fixes 12 weeks from first enrolment to last follow-up (H L20), which bounds the
  "Recruitment and intervention" row but is neither that row's definition nor a figure in months;
  I did not convert it. Please give the three figures.
- **Q4 — Budget (T36, T40).** No amount, currency or line item exists in the state files.
- **Q5 — Title (T1).** The project name "Harbor" is left out (rail b: plain words, never internal
  codes). If Greywick already knows the study by that name, say so and it can go in. The title also
  drops H_001's verb "reduces" so as not to assert the result; confirm the wording.

## 5. Excluded on purpose

- **H_002** (H L28–44): `status: proposed`, `proposer: fable`, `decided_by: ""` — not quotable
  (rail g), and D_001 (D L20–23) keeps any extension out of every outward document until the pilot
  reads out. None of its figures (90 participants, 30 per arm, three sites, 16 weeks, a
  stretching-plus-education arm) appears in the draft. Checked by grep.
- **Internal identifiers** "Harbor", "H_001", "D_001": absent from the draft (rail b).
- **Evidence file** `notes/site-visit-2026-10.md` (H L15): not in the bundle; not cited.
- **Discipline card:** none in use (O L26); the field (occupational health, O L27) has no card, so
  register and length follow the template alone (rail e).

## 6. Rails

- (a) template verbatim, slots only — §3 above.
- (b) plain words, no codes — §5.
- (c) title from the operator's entry — RS L23–25.
- (d) first page states the whole case: header + Summary carry partner, institution, investigator,
  intervention, comparator, 48 participants, 12 weeks, voluntary participation. Background is
  blank on that page until Q2 is answered.
- (e) template register; no card — §5.
- (f) operator's full read before it leaves; this role sends nothing.
- (g) every number from H_001 (accepted, decided by human) or RS, worded as a plan; administrative
  figures absent → blank + Q1, Q3, Q4.
- (h) §9 figure in the reading copy (§1); fresh-context check pending.

## 7. Hand-offs

- **Operator:** full read of `PROPOSAL.md`; answers to Q1–Q5 into `RESEARCH_STATE.md`.
- **Fresh-context checker** (SCIENTIFIC_RULES §9): verify the §1 figure for added claims and dropped
  conditions; the main session dispatches it (this role authored the figure and may not check it).
- **repo-maintainer:** committing, when the operator wants it — not done here (task: do not commit).
