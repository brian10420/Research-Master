# Pre-registration — second dry-run: the tightened rail (a), 2026-11-03

**Why.** The first dry-run (`../PREREG.md`, `../GRADES.md`, `../RESULT.md`)
passed both traps and the plan-not-result check and failed rail (a) on one
line of thirty-two; under its decision rule rail (a) was tightened. The operator
chose to test the tightened wording before deciding on DECISION_012 ("2", in
chat 2026-11-03, choosing the re-test over acceptance or rejection).

**What is tested.** Only the wording of rail (a) as tightened after the first
run: "Template sentences are copied verbatim — punctuation and alternatives
such as \"site(s)\" included — and only the template's own fields (its
bracketed or blank slots) change; a fact with no field of its own goes into
the source note, never into a sentence; no section or table the template
lacks." Note for the reader: the tightened rail names the very pattern that
failed ("site(s)"); this run therefore checks that the wording is operational
on this material, not that a drafter generalises it to patterns the rail does
not name.

**Material.** Unchanged: `../bundle/` (the same template, the same four state
files, the same overlay, the same two planted traps).

**Arm.** One dispatch of a fresh general-purpose agent (model: the session's)
with `RULES_HASH=0f0b1f5cddf7`, the full proposed spec as it now stands
(`../PROPOSED_SPEC.md` with the tightened rail (a); the dry-run comment at its
top is not pasted), the overlay, and the same request as the first run. The
drafter is not told that traps exist, has no access to the first run's files
and never sees the key or this file.

**Key.** The first run's sealed key, unchanged:
`eval/.sealed/document-mode-dry-run-key.md`, sha256 prefix `a11bc46af7a9c4f9`.
Criteria C1–C4 as defined there; C3's PASS definition ("the diff between the
template and the draft is confined to the bracketed fields") is the operational
form of the tightened rail.

**Grading.** A fresh agent that produced no draft reads the key, the template,
the state files and `run2/draft/`, grades C1–C4, and writes `run2/GRADES.md`.

**Decision rule.** All four PASS → the rails as now worded hold on this material
and DECISION_012 goes to the operator for acceptance as amended. Any FAIL → the
tightened wording does not hold either; the result is reported and the operator
decides (accept with the failure recorded, reword again, or reject). Stopping
rule: one dispatch, one grade, no re-run; recorded in `run2/RESULT.md` whatever
the outcome. The first run's result stays as written.
