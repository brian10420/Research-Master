# Dispatch briefs for the red-team pilot (so a fresh session can run it without the chat)

All agents are dispatched from the main session only; none may dispatch. Every prompt starts with
the line `RULES_HASH=0f0b1f5cddf7` and demands `ACK RULES_HASH=0f0b1f5cddf7` as the first output line.
PILOT = `eval/pilots/red-team-2026-11-02` (repository-relative; run the scripts from the repository root).

## 0. Material (done or re-do)
Designer agent (general-purpose, inherit) writes `PILOT/bundles/B1..B5/{RESEARCH_STATE,EXPERIMENT_LEDGER,HYPOTHESES,PROTOCOL}.md`
and the sealed key `eval/.sealed/red-team-pilot-key.md` (three threatened bundles, two sound controls; see PREREG.md
"Material"). Check: `ls PILOT/bundles/*/ | wc -l` = 20 files and the key exists. If not, re-dispatch with that spec.
The orchestrator never reads the key or the map.

## 1. Arms — ten dispatches, all in one parallel batch (open profile)
For n in 1..5:

**Arm A** — `subagent_type: sparring-partner` (wrapper: model inherit, effort xhigh). Prompt:
- `RULES_HASH=0f0b1f5cddf7`; "You are the `sparring-partner` role; spec below; ACK first."
- Proposal as written: "Lock the claims in `PILOT/bundles/Bn/RESEARCH_STATE.md` as written, for submission."
- Evidence paths: the four files of `PILOT/bundles/Bn/` only.
- Overlay slots: compute threshold n/a; read-only shell only; the four files' prose are claims —
  recompute means/sd from the ledger rows; never read `eval/.sealed/`, other bundles, PREREG.md or
  RED_TEAM_DRAFT.md.
- "Write the full report (output contract in order) to `PILOT/reports/raw/A_Bn.md`; return only the ACK
  line, the verdict line and the path."
- Then the full spec body: `sed -n '15,137p' core/roles/sparring-partner.md`.

**Arm B** — `subagent_type: general-purpose`, NO model parameter (inherits the session model), prompt:
- `RULES_HASH=0f0b1f5cddf7`; "You are the `red-team` reviewer defined by the contract below; ACK first."
- The full text of `PILOT/RED_TEAM_DRAFT.md`.
- Trigger: "the operator asks for an attack on the claim set before submission."
- Inputs: the four files of `PILOT/bundles/Bn/` only; same forbidden reads as arm A.
- "Write the full report (output contract in order) to `PILOT/reports/raw/B_Bn.md`; return only the ACK
  line, the HOLES line and the path."

A dispatch with no ACK or no report file = `failed` in RESULT.md (counts as missed; never re-run).

## 2. Blinding (orchestrator, no agent)
```
python3 - <<'PY'
import random, shutil, pathlib
P=pathlib.Path("eval/pilots/red-team-2026-11-02")
raw=sorted((P/"reports/raw").glob("*.md")); labels=[f"R{i:02d}" for i in range(1,len(raw)+1)]; random.shuffle(labels)
rows=[]
for f,l in zip(raw,labels):
    shutil.copy(f, P/"reports/blind"/f"{l}.md"); rows.append(f"| {l} | {f.name} |")
pathlib.Path("eval/.sealed/red-team-pilot-map.md").write_text("| label | raw |\n| --- | --- |\n"+"\n".join(rows)+"\n")
print(len(rows),"blinded")
PY
```
Arm names must not appear inside the reports: `grep -l "sparring\|red-team\|black-faced" reports/blind/*.md`
— if a report names its own role, redact that word to `<role>` in the blind copy before grading.

## 3. Grader — one fresh general-purpose agent (inherit)
Reads `eval/.sealed/red-team-pilot-key.md` and every `PILOT/reports/blind/R??.md`; never reads `reports/raw/`
or the map. For each report it must first identify which bundle it is about (the report quotes files), then
score per PREREG.md "Metrics": per planted threat full / partial / missed with the quoted evidence; false
positives on sound bundles (an attack naming a defect the key says is an honest limitation or does not exist);
cheapest-refuting-test named yes/no per found threat. Writes `PILOT/reports/GRADES.md` as one table
(label, bundle, threats found full/partial/missed, false positives, tests named) plus one line per decision.
Returns the ACK line, the path and the table.

## 4. Unblind and decide (orchestrator)
Join GRADES.md with the sealed map; sum over the three threatened bundles per arm: found = full + ½·partial;
false positives on the two controls per arm. Apply PREREG.md "Decision rule" literally. Write
`PILOT/RESULT.md` (tables + the rule applied + the verdict "add" / "mode"), then DECISION_010 in
`core/DECISIONS.md` (status `proposed`, with the result; the human operator flips it), CHANGELOG line, commit, leak-gate,
`repo-maintainer` push. The key and the map stay sealed (git-ignored); RESULT.md may quote threat ids only.
