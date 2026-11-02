# Changelog

Versions follow `.claude-plugin/plugin.json`; bump both manifests together.

## 0.5.1 — 2026-11-02

- **DECISION_010 accepted** by the human operator 2026-11-02 (in chat). The
  verdict `mode` takes effect: `sparring-partner` gains the **claim-set mode**
  in "When it is dispatched" — after a surprising result, or on request before
  submission, with the proposal "lock the claims in `RESEARCH_STATE.md` as
  written, for submission" and the state files as the evidence paths; the
  roster line and both READMEs' team tables say so. No new role; the roster
  stays at fourteen. Wrappers regenerated; `core/SCIENTIFIC_RULES.md` unchanged
  (RULES_HASH stays `0f0b1f5cddf7`).
- **Character illustrations** (`assets/roles/illustrations/`): 512 px PNG
  renders of the operator's hand-drawn 1254 px originals (the originals and
  their archive stay outside version control) — one expressive bear per role
  plus `critical-auditor`, the black bear drawn for the red-team candidate,
  which after DECISION_010 has no role behind it and is kept as art. Not
  claim-bearing, so no claims table; noted in `assets/README.md`.
- Release note for 0.5.0's push: the public leak-gate reported seven hits, all
  in the pilot's synthetic bundles and reports and all coincidental numeric
  matches (two Student-t critical values, one arithmetic difference, one
  synthetic standard deviation, one invented ledger row). The graded record was
  not edited; the operator reviewed the lines and said "push anyway".
- Manifests 0.5.1.

## 0.5.0 — 2026-11-02

- **Red-team pilot run and decided** (DECISION_010, proposed): a fifteenth role
  — an adversarial critic of the claim set — was tested against
  `sparring-partner` on five synthetic mini-projects (three with planted threats
  to validity, two sound controls; key sealed before dispatch), ten dispatches
  in one batch, same model and effort, reports blinded and graded against the
  key by an agent that ran no arm, decision rule pre-registered. Result: both
  arms found all 7 planted threats (7 / 7, every grade full); the candidate made
  7 false positives on the two sound controls against 0 for `sparring-partner`,
  at about 1.6× the tokens and 3.5× the text. Verdict `mode`: no new role; on
  acceptance the two residual triggers ("after a surprising result", "on
  request") become a named dispatch mode of `sparring-partner`. Record in
  `eval/pilots/red-team-2026-11-02/` (`PREREG.md`, `RESULT.md`,
  `reports/GRADES.md`, bundles, raw and blind reports); the key and the label
  map stay sealed in `eval/.sealed/`. Two protocol deviations are written down
  in `RESULT.md` §5 (a designer agent survived a session reset and its material
  was discarded and rewritten; the arms' one-line verdicts reached the
  orchestrator before grading); neither touches a grade.
- **Role bears** (`assets/role-bears.svg`; `assets/roles/<role>.svg` with 128 px
  PNG renders; `assets/roles/symbols.svg`): fourteen icons, each the logo's head
  without the mortarboard plus one prop per role, on a contact sheet with the
  two posture bands and the gate. The §9 claims table is the sixth section of
  `assets/README.md`. Fresh-context §9 check: `VERDICT: PASS-WITH-FIXES` — 37
  claims rows checked; 9 findings (1 added claim, 4 dropped conditions, 1 stale
  line reference, 0 misquotes, 3 unsourced labels, 0 sheet/icon mismatches),
  every fix applied verbatim: four purpose labels re-worded so a dropped
  condition is kept (`/conductor` "dispatch each stage", `sparring-partner`
  "premise audit before a decision", `blind-reviewer` "peer review of an
  anonymized bundle", `/study-coach` "teach you your project"), the gate legend
  now reads "isolated; regression-guardian only" (the gate is a mark on an
  isolated role, not a third posture), three legend/note rows gained their
  sources, the table's commit pin updated. After the check, the sheet's footer
  pointer moved from the standalone claims file to `assets/README.md` because
  the file was merged (path only; no claim changed).
- **READMEs (EN + zh-TW):** the role icons as a first column of both "Meet the
  team" tables and the bears contact sheet under the team map. Heading and
  details parity kept (8 / 10).
- **`tools/sync.py`:** the isolated-role list in `AGENTS.md` is rendered from
  the roster; the hand-typed list it replaces had omitted `sparring-partner`
  since the role was added (found by the sparring audit of the red-team
  proposal). `AGENTS.md` regenerated.
- Pilot hygiene for the public tree: `DISPATCH_BRIEFS.md` uses
  repository-relative paths and "the human operator".
- Manifests 0.5.0; `core/SCIENTIFIC_RULES.md` unchanged (RULES_HASH stays
  `0f0b1f5cddf7`).

## 0.4.1 — 2026-11-02

- **Effort policy amended** (DECISION_009, accepted by the human operator
  2026-11-02 in chat): `experiment-runner` and `repo-maintainer` move from
  Opus @ `medium` to Opus @ `xhigh` — the operator's standing rule is that Opus
  never runs below `high`, and he chose `xhigh` for the plugin's two operations
  roles. Wrappers regenerated; the team map's two badges and the roster
  doctrine sentence updated; `SCIENTIFIC_RULES.md` unchanged (RULES_HASH stays
  `0f0b1f5cddf7`). Manifests 0.4.1.

## 0.4.0 — 2026-11-02

- **Plan-tier dispatch profile** (DECISION_008, proposed): new
  `core/dispatch_policy.yaml` with two profiles — `guarded` (free, pro, max,
  team-standard, team-premium: one isolated dispatch at a time; workflows small)
  and `open` (ultra, enterprise, api: independent stages in parallel, the number
  set by quality and dependencies under the harness ceiling; workflows
  unconstrained). The overlay template gains `## Runtime budget` (`Plan tier:`,
  optional `Dispatch profile override:`); `conductor` gains hard rule 13 (derive
  the profile, state it in the first SLIM line or dashboard, ask once when the
  slot is missing and run `guarded` meanwhile, never edit the overlay), the
  input-contract item, the procedure wording and the overlay slot. Unchanged
  under both profiles: the main session is the only dispatcher, committer and
  publisher; agents never dispatch; one deliverable per agent; the idle limit;
  spec + hash in every dispatch; blind-review isolation. "Open never means
  free": every plan keeps a weekly limit. Both adapters carry the mapping,
  rendered from the policy file (`CLAUDE.md` adds the harness ceilings
  `CLAUDE_CODE_MAX_CONCURRENT_SUBAGENTS` / `…_SPAWN_DEPTH`). Background: the
  one-at-a-time practice dated from the rolling 5-hour window; the operator
  asked on 2026-11-02 to lift it only for plans without that window and only
  after the plugin asks which plan is in use. Sources in DECISION_008.
- **Wrapper audit against the official Claude Code guidance (read 2026-11-02):**
  - `blind-reviewer`'s wrapper carries `omitClaudeMd: true` (role frontmatter
    `harness_instructions: omit`): subagents load the CLAUDE.md hierarchy by
    default, which would carry a consuming project's story into a
    context-blind reviewer.
  - Adapter wording corrected: in Claude Code the agent body *is* the
    subagent's system prompt; the embedding rule stays for other harnesses and
    as defence in depth (SCIENTIFIC_RULES §3 unchanged).
  - `skills/` and `agents/` at the repository root are now regular,
    byte-identical generated copies of `.claude/skills` and `.claude/agents`
    (the plugin directory accepts no symlinks); `tools/sync.py` writes 66
    adapters instead of 34, `tools/check_drift.py` checks and orphan-scans both
    trees. `CLAUDE_SKILL_DIR` for the installed plugin is unchanged.
  - The five isolated agents carry a `color:` (guardian `yellow`, matching the
    amber gate in the figures; cosmetic).
  - Mechanical checklist over the 14 specs and 14 wrappers: rule 0 canary,
    `neighbours:`, overlay slots and boundaries present, no tier words;
    descriptions 214–388 characters (limit 1,536); wrappers ≤ 147 lines
    (limit 500). Zero findings; no change needing a `SCIENTIFIC_RULES` edit.
  - `claude plugin validate .` passes; `claude plugin details research-kernel`
    lists 9 skills + 5 agents (always-on cost ≈ 1.5 k tokens per session).
- **README rewritten for first-year graduate students** (English) and
  **`README.zh-TW.md`** added (Traditional Chinese, same headings, tables and
  figures): is this for you → your first ten minutes → meet the team by need →
  the workflows → why it will not let you cherry-pick → pick your plan tier;
  the architecture, layout, compile layer, effort policy, blind review,
  no-plugin install and provenance notes are folded, not deleted (§9).
- **Figures** (`assets/`), all hand-written SVG, one §9 claims table per figure in
  `assets/README.md`: new **logo** (`logo.svg`; `logo.png` 1024 px is the plugin
  icon; `logo-256.png` heads both READMEs) — a polar bear in a crooked
  mortarboard; **`team-map.svg` v3** (fourteen roles: nine skills, five
  subagents; badges exactly as `effort_policy.yaml`; the dispatch-profile tag on
  the bus; every role tagged with its "never does" cell; closes the 0.3.0 known
  gap); **`team-flows.svg` v2** (six lanes: code/experiment with the sparring
  gate, paper with the anonymized-bundle boundary, present, learn, the
  conductor's pipeline with checkpoint classes and the guarded/open fork, and a
  stuck lane redrawn so it no longer chains "stuck → /effort max", which
  contradicted the roster's "never by it got stuck"; v1's unsourced "PDF only"
  and "otherwise the deck ships at step 4" dropped); new **`day-one.svg`** (the
  first ten minutes) and **`plan-tier.svg`** (plan → profile → what never
  changes). Fresh-context §9 check (figures + both READMEs as the gloss): `VERDICT:
  PASS-WITH-FIXES` — 219 claims rows checked; 23 findings (8 added claims, 10
  dropped conditions, 2 EN↔zh-TW mismatches, 1 stale line reference, 2
  unsourced aria-labels), every fix applied verbatim. Two conflicts the check
  surfaced were resolved at the source rather than on the figure: the overlay
  template no longer calls itself the only home of project facts (the state
  files share it), and conductor rule 13 now names the optional `Dispatch
  profile override:` line the template already offered. The plan-tier figure
  carried "DECISION_008 (proposed)" in its kicker until the decision was
  accepted the same evening. One checker wording for the `repo-maintainer` cell was
  replaced by the roster's exact words ("forced push unasked",
  `core/roles/README.md`) because the public leak-gate flags the checker's
  hyphenated form; the condition is unchanged.
- Manifests 0.4.0; `plugin.json` gains `icon` (`assets/logo.png`, 1024 px).
  `core/SCIENTIFIC_RULES.md` unchanged (RULES_HASH stays `0f0b1f5cddf7`).
- Listed, not done: an `evals/` suite for `claude plugin eval`; a directory
  submission; the operator's own harness settings (outside the kernel).
- Decision accepted by the human operator: DECISION_008 on 2026-11-02 (in
  chat); the plan-tier figure's kicker now reads "accepted".

## 0.3.0 — 2026-10-30

- **New role `conductor`** (shared-context, 14th role; DECISION_007, accepted):
  runs a multi-stage pipeline from the operator's session and does no
  substantive work itself — every stage goes to the role, script or workflow
  that does it; hat switches announced and acknowledged (§3.4); condensed
  returns; dispatch inputs limited to each role's input contract; one
  deliverable per agent; mechanical work as scripts, hooks or workflow steps;
  the main session is the only committer, publisher and dispatcher. Checkpoint
  classes MANDATORY (overlay-named, never skipped) / FULL (after 4 SLIM
  continues, a failed gate, an override, or before a deviation from the
  pipeline) / SLIM (one line, default runs); a decision dashboard (key points →
  ≤ 3-line summary → numbered options with a default) with a five-item
  self-check line; SOFT-GATE overrides only on the operator's choice with a
  logged reason, HARD-STOP never; verdicts verbatim; pipeline state in the
  project's tracker file. Model `inherit`, effort session-default; no effort-policy entry.
- **`core/disciplines/README.md`**: the discipline-card format (evidence
  conventions, terminology, what reviewers in the field ask, ethics and consent
  norms, the quantitative / qualitative / mixed switch); no cards yet.
- `research-mentor` and `paper-writer` gain `conductor` as a neighbour and one
  Boundaries line each; the roster gains the row, a boundary bullet and a
  **Pipeline** flow; the overlay template gains a `### conductor` slot and a
  `## Disciplines` section.
- Manifests 0.3.0: fourteen roles (9 in-session skills + 5 isolated
  subagents). `core/SCIENTIFIC_RULES.md` is unchanged (RULES_HASH stays
  `0f0b1f5cddf7`); adapters regenerated with `tools/sync.py` (34);
  `tools/check_drift.py` clean.
- Decision accepted by the human operator: DECISION_007 on 2026-10-30.
- Known gap: `assets/*.svg` still show the 0.1.0 team.

## 0.2.1 — 2026-10-12

- **SCIENTIFIC_RULES §9 "Operator-facing documents — draw before you narrate"**
  (DECISION_006, accepted): figures lead wherever the content is a flow, an arm
  comparison, a decision rule, a noise or power statement or a schedule; long
  normative text folded, never deleted; a figure never replaces its derivation;
  every figure label traced to a source line; an independent fresh-context check
  of figures and glosses, applied verbatim.
- `research-mentor` hard rule 8 applies §9 to what the mentor puts in front of
  the operator; the overlay template gains a "Reading surface" slot.
- `marketplace.json` description corrected: thirteen roles (8 in-session skills
  + 5 isolated subagents) since 0.2.0; it still said twelve.
- Adapters regenerated with `tools/sync.py` (RULES_HASH changes);
  `tools/check_drift.py` clean.
- Decisions accepted by the human operator: DECISION_004 and DECISION_005
  (filed with 0.2.0) on 2026-09-30, DECISION_006 on 2026-10-12.

## 0.2.0 — 2026-09-28

- **New role `sparring-partner`** (isolated, 13th role; DECISION_005, proposed):
  premise audit of a decision before it is made — verdict label, premise audit
  with file:line evidence, strongest alternative including doing nothing, kill
  criteria, regret list; data-contact rule; rebuttal scale with "one push, then
  retreat"; never a veto. `research-mentor` hard rule 7 = the sparring gate (the
  answer opens with the audit's verdict). Roster row and boundary added.
- **Effort policy amended** (DECISION_004, proposed): `experiment-runner` and
  `repo-maintainer` → `opus` @ `medium` (were `sonnet` @ `low`);
  `regression-guardian` and `sparring-partner` → `xhigh`. The documented claim
  that "`model: inherit` does not inherit effort" is corrected: a wrapper without
  an `effort:` key inherits the session effort.
- Adapters regenerated with `tools/sync.py`; `tools/check_drift.py` clean.
- Known gap: `assets/team-map.svg` and `assets/team-flows.svg` still show the
  0.1.0 team (twelve roles, 0.1.0 badges); the README says so.

## 0.1.0 — 2026-09-15

First installable release (DECISION_003).

- **Plugin packaging:** `.claude-plugin/plugin.json` + `marketplace.json`; the
  repository is its own marketplace (`source: "./"`). Root `skills/` and
  `agents/` symlink to the generated `.claude/` wrappers.
- **Self-contained wrappers:** each skill directory now carries `role.md` and
  `RULES.md` generated by `tools/sync.py`; isolated agents inline their spec.
  No wrapper resolves a project-root path any more.
- **Project overlay:** `templates/research-kernel.overlay.template.md` →
  `.claude/research-kernel.overlay.md` in the consuming project, injected by
  every skill; the only place project facts belong.
- **Effort policy:** `core/effort_policy.yaml` drives enforced `effort:` /
  `model:` keys (task-shape doctrine; no role pins `max`).
- README: "Install in 30 seconds"; copy-install kept as the no-plugin path.
