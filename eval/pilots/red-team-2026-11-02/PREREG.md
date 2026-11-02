# Pre-registration — red-team pilot (DECISION_010 condition), 2026-11-02

**Question.** Does a dedicated adversarial critic contract (`red-team`, the
black-faced bear) find holes in a project's claim set that a `sparring-partner`
dispatch pointed at the same claim set does not?

**Arms.** Same model (`inherit`, the dispatching session's model), same effort
(the sparring wrapper pins `xhigh`; the red-team draft has no wrapper and
inherits the session effort, which is `xhigh`), same inputs, same return format.
- **A (baseline):** `sparring-partner`, canonical spec, proposal = "lock the
  claims in `RESEARCH_STATE.md` as written, for submission", evidence paths =
  the bundle's files.
- **B (candidate):** the red-team draft contract (`RED_TEAM_DRAFT.md` in this
  directory; the audit's amended version: isolated, no manuscript, triggers
  "after a surprising result / on request", sparring's data-contact rule),
  same bundle, asked for ranked attacks each with the cheapest refuting test.

**Material.** Five synthetic mini-projects (`bundles/B1..B5`), generic ML, no
real project facts. Three carry planted threats to validity (one to three
each, from: checkpoint selection on the test split; missing or weak baseline;
a single seed reported as "the result"; metric mismatch between claim and
ledger; a claim stronger than its evidence; deleted or omitted failed runs;
an under-powered comparison), two are sound controls. The planted list is
written by a designer agent and sealed in `eval/.sealed/red-team-pilot-key.md`
(git-ignored) **before** any arm is dispatched; neither arm nor the orchestrator's
dispatch prompts name the threats.

**Blinding.** Each arm writes one report per bundle to `reports/raw/`. The
orchestrator renames the ten reports to random labels `R01..R10` in
`reports/blind/` and seals the map in `eval/.sealed/red-team-pilot-map.md`. A
fresh-context grader that ran no arm scores every blind report against the key.

**Metrics (per report).** (1) planted threats found: full / partial / missed,
per threat; (2) false positives on the two sound controls (an attack that
names a defect the bundle does not have); (3) for each found threat, whether a
concrete cheapest refuting test is named (yes/no).

**Decision rule (pre-declared).** Sum over the three threatened bundles. Add the
`red-team` role (accept DECISION_010 as "add") only if arm B finds strictly more
planted threats (full + ½ partial) than arm A **and** B's false positives on the
controls are not greater than A's. A tie, or A ≥ B, keeps the roster at
fourteen; the two residual triggers are then documented as a dispatch mode of
`sparring-partner`. Refuting-test quality is reported, not decisive.

**Stopping rule.** Exactly one run of ten dispatches; no re-dispatch of an arm
whose report is weak; a dispatch that fails (no ACK, no report) is recorded as
failed and counts as "missed" for that arm. The result is recorded in this
directory (`RESULT.md`) and summarised in DECISION_010 whichever way it goes.
