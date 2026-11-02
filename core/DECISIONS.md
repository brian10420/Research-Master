---
schema: research-os/decisions/v1
scope: cross-project (this repository). Per-project decisions live in the project repo's own DECISIONS.md.
entry_template: |
  id: DECISION_<NNN>
  title: <one line>
  date: YYYY-MM-DD
  proposer: human | fable | astra | other-model:<name> | joint:<list>
  decided_by: human
  status: proposed | accepted | rejected | superseded
  evidence: [<paths/urls>]
  supersedes: ""
  superseded_by: ""
  context: <what prompted the decision>
  decision: <what was decided, in one paragraph>
  consequences: <what changes because of it; what it forbids>
  alternatives_rejected: [<one line each>]
---

# Decisions

Append-only. A reversed decision is a new entry that `supersedes` the old one.
Only a human moves an entry to `accepted` or `rejected`.

## Entries

```yaml
id: DECISION_000
title: Adopt the provider-neutral Research OS architecture
date: 2026-09-04
proposer: joint:[fable-5, gpt-5.6]   # dual-model cross review, 2026-09-04
decided_by: human
status: accepted
evidence: [MIGRATION_AUDIT.md, core/SCIENTIFIC_RULES.md]
supersedes: ""
superseded_by: ""
```

**Context.** The research team existed as a Claude-specific skill collection: role
files that only one vendor's runtime could load, with volatile project facts baked
into the role bodies and a "memory wins" clause to paper over staleness.

**Decision.** The research methodology and its decision history are the canonical
asset. They live in `core/` (rules, state schemas, role specs) under git. Every AI
vendor — Claude Code, Codex, future agents — is a replaceable runtime reached
through *generated* adapter files (`CLAUDE.md`, `AGENTS.md`, thin wrappers) produced
by `tools/sync.py` from `core/`. Adapters are never hand-edited; drift is blocked by
`tools/check_drift.py`.

**Consequences.**
- Cross-project rules and roles live in this repository; per-project state
  (`RESEARCH_STATE`, `DECISIONS`, `EXPERIMENT_LEDGER`, `HYPOTHESES`,
  `FAILED_IDEAS`, `OPEN_QUESTIONS`) lives in each project repository, seeded from
  `templates/project-state/`.
- **Two-track inversion (owner ruling 2026-09-04).** Before: a project's private
  `.claude/` was canonical and this public repo was derived by manual scrub. After:
  `core/` here is canonical for cross-project rules and roles; a project's
  `.claude/` becomes a project-specific overlay. The publication hygiene does not
  relax: the private leak-gate must print CLEAN before any public push, and
  project-specific facts never enter `core/`.
- Role specs carry no volatile project facts; those move to the project's state
  files (see DECISION_001).
- Subagents must receive the full role spec and `RULES_HASH` inside the task prompt
  (canary handshake, SCIENTIFIC_RULES §3).

**Alternatives rejected.**
- Keep one vendor's format as canonical and translate outward — leaves the
  methodology hostage to that vendor's file conventions.
- Store rules in model memory — violates principle 1 (memory is recall, not
  authority).

---

```yaml
id: DECISION_001
title: Supersede the "memory wins" doctrine — git state is authoritative, memory is recall
date: 2026-09-04
proposer: fable          # raised in the Phase 0 audit as Conflict 3
decided_by: human        # ratified at P0 approval, 2026-09-04
status: accepted
evidence: [MIGRATION_AUDIT.md, docs/design-notes.md]
supersedes: ""           # supersedes the doctrine paragraph in docs/design-notes.md and the former RESEARCH_TEAM.md
superseded_by: ""
```

**Context.** The former roster and five role files said: "when a role file and
project memory disagree, trust memory and flag the drift." That rule existed
because role files baked in facts that rotted between sync passes.

**Decision.** Git, Obsidian, and the experiment ledgers are authoritative; model
memory is recall-only. Role specs carry no volatile facts at all — those live in
the project's `RESEARCH_STATE.md` and `EXPERIMENT_LEDGER.md`, which are read at
session start. When memory contradicts git state, memory loses: the
memory-curator proposes a `memory_forget`, never a silent edit, and never an edit
to `core/`.

**Consequences.** Every "memory wins" clause is removed from role specs; the
design-notes paragraph is annotated as superseded (kept for history). Staleness
is now a *state-file* problem with a git diff, not a memory problem with no
audit trail.

**Alternatives rejected.**
- Keep "memory wins" but sync more often — still no audit trail for what memory
  asserted.

---

```yaml
id: DECISION_002
title: Freeze eval/RUBRIC.md v1 and eval/PROTOCOL.md v1 as the pre-registered evaluation
date: 2026-09-04
proposer: fable          # drafted in Phase 6 of the migration
decided_by: human        # ratified 2026-09-05
status: accepted
evidence: [eval/RUBRIC.md, eval/PROTOCOL.md]
supersedes: ""
superseded_by: ""
```

**Context.** The evaluation scaffold (five tasks, weights 20/30/20/20/10,
anchors 1–5, two comparisons graded blind) exists; results are meaningful only
against a frozen rubric.

**Decision (proposed).** Accepting this entry freezes rubric v1 and protocol
v1. Any later change requires a new entry that supersedes this one and a
version bump; results across versions are never merged.

**Consequences.** Planted-defect lists for T1/T2/T4 must be sealed before the
first run; repetitions per cell are declared before the first run.

**Alternatives rejected.** Grading against an evolving rubric — makes every
comparison across runs invalid.

---

```yaml
id: DECISION_003
title: Ship research-kernel as a self-contained Claude Code plugin; project facts live in an overlay; effort is chosen by task shape
date: 2026-09-15
proposer: fable          # drafted from the operator's request to make installation as easy as a marketplace plugin
decided_by: human
status: accepted
evidence: [.claude-plugin/plugin.json, .claude-plugin/marketplace.json, core/effort_policy.yaml, templates/research-kernel.overlay.template.md, tools/sync.py]
supersedes: ""
superseded_by: ""
```

**Context.** Installing the kernel took five manual steps (clone, vendor `core/`,
copy `CLAUDE.md` + `.claude/`, run `sync.py`, write the overlay into the
project's instruction file), because the generated skill wrappers resolved
`core/roles/<role>.md` relative to the project root. A Claude Code plugin lives
in an immutable, wholesale-replaced cache directory, so that path — and any
hand-edit inside the plugin — breaks on install or update.

**Decision.** (1) Every generated wrapper is self-contained: a skill directory
carries `SKILL.md` + `role.md` (verbatim spec) + `RULES.md` (condensed rules)
and loads only from its own directory; an isolated role's agent file inlines
its spec. `skills/` and `agents/` at the repository root are symlinks to
`.claude/`, and `.claude-plugin/{plugin,marketplace.json}` make the repository
its own marketplace and plugin (`/plugin marketplace add <owner>/<repo>` →
`/plugin install research-kernel@research-kernel`). (2) Project-specific facts
live in the consuming project's `.claude/research-kernel.overlay.md` (template
in `templates/`), injected by every skill at load time and passed to isolated
roles in the dispatch prompt; nothing inside the plugin is ever edited.
(3) `core/effort_policy.yaml` is the single, data-driven source of the
enforced Claude Code `effort:` / `model:` keys: closed verification roles at
`xhigh`, mechanical roles at `low` on a smaller model, everything else inherits;
no role pins `max`; the operator raises the *session* effort for an
unbounded-judgement step; "stuck" is handled by changing method, not effort.

**Consequences.** The copy-install path keeps working and no longer needs
`core/` in the project. `tools/check_drift.py` covers the new generated files.
The `runtime:` block in each spec stays a provider-neutral recommendation; the
policy file is the Claude Code enforcement, and the two may differ on purpose.
Public pushes still require the private leak-gate to print CLEAN.

**Alternatives rejected.**
- Keep project-root-relative wrappers and document "vendor `core/`" — breaks
  inside the plugin cache and on every update.
- Hand-edit `effort:` into wrappers inside the plugin cache (what a third-party
  plugin forces its users to do) — silently reverted by the next update.
- Pin `max` on the reviewer roles — the closed-task curve saturates at `xhigh`;
  `max` is a session-level decision for long-horizon judgement steps only.


---

```yaml
id: DECISION_004
title: Amend DECISION_003's effort policy — operations roles on the strong model at medium; the guardian and the sparring partner at xhigh
date: 2026-09-28
proposer: fable          # ported from the source project's routing pass
decided_by: human        # accepted by the human operator 2026-09-30
status: accepted
evidence: [core/effort_policy.yaml, README.md]
supersedes: ""           # amends point (3) of DECISION_003 only; points (1)–(2) stand
superseded_by: ""
```

**Context.** DECISION_003 (3) pinned the two operations roles to a smaller model
at `low` ("never needs frontier reasoning") and left `regression-guardian`
unlisted. In the source project the operator reversed the first half on
2026-09-28: operations errors are the expensive ones — a launch pattern that
matches the wrong process, a protected path in a push — and they come from
missed context, not from hard reasoning. The same pass pinned the guardian,
because certify-or-reject is a closed, single-answer judgement of exactly the
shape the policy already puts at `xhigh`. Separately, the documentation claim
that "`model: inherit` does not inherit effort" was checked against the live
Claude Code docs and is wrong: a wrapper without an `effort:` key inherits the
session effort.

**Decision (proposed).** `experiment-runner` and `repo-maintainer`: `model:
opus`, `effort: medium`. `regression-guardian` and the new `sparring-partner`
(DECISION_005): `effort: xhigh`, model inherited. The doctrine sentence becomes
"closed verification at `xhigh`, operations at `medium` on the strong model,
everything else inherits". The effort-inheritance sentence is corrected in
`core/effort_policy.yaml` and `README.md`. The provider-neutral `runtime:`
recommendations in the role specs are unchanged (a smaller model stays a valid
recommendation for other runtimes; policy and recommendation may differ on
purpose, DECISION_003).

**Consequences.** Operations dispatches cost more tokens per call than under
0.1.0. No role pins `max`; the operator-held session switch is unchanged. The
team-map diagrams still show the 0.1.0 badges until they are redrawn.

**Alternatives rejected.**
- Keep `sonnet` @ `low` for operations — cheapest, but the costly failures of
  those roles are context misses that a smaller model makes more often.
- Pin `xhigh` on operations — the reasoning is shallow; `medium` suffices once
  the model is strong.

---

```yaml
id: DECISION_005
title: Add sparring-partner — an isolated premise auditor for decisions, dispatched from a research-mentor gate
date: 2026-09-28
proposer: fable          # built and tested in the source project's optimization week
decided_by: human        # accepted by the human operator 2026-09-30
status: accepted
evidence: [core/roles/sparring-partner.md, core/roles/research-mentor.md, core/roles/README.md]
supersedes: ""
superseded_by: ""
```

**Context.** No role audited the premises of a *decision*. The blind reviewer
judges manuscripts and the guardian judges code changes; proposals for new
experiments or claim changes were discussed only in the shared-context mentor
lane, where the auditor shares the proposer's assumptions. In the source
project most recorded reversals were premise failures that could have been
found in the files before any compute was spent.

**Decision (proposed).** A thirteenth role, `sparring-partner` (isolated;
Read / Grep / Glob / read-only shell / web). It receives the proposal as
written plus its evidence paths — never the proposer's reasoning — and returns
a verdict (`PROCEED` / `PROCEED-WITH-CHANGES` / `RETHINK`), a premise audit with
file:line or command evidence, the strongest alternative including doing
nothing, kill criteria with the cheapest disconfirming test, a regret list and
what is solid. It follows a data-contact rule (it computes nothing a
not-yet-contacted readout will report), a rebuttal scale with "one push, then
retreat", and never vetoes. `research-mentor` gains a sparring gate (hard rule
7): above the project's compute threshold, at the design phase of a large plan,
and after a quick operator–mentor agreement on direction, it dispatches the
role and opens its answer with the audit's verdict.

**Evidence and its limits.** The source project ran a pre-declared RED/GREEN
test (three planted-flaw scenarios, two sound controls, one pressure rebuttal;
three repeats per cell). A fresh, equally isolated generic reviewer on the same
model already caught every planted flaw, so the pre-declared "strictly better
than the baseline" criterion was **not met** (a tie at the ceiling). What the
role measurably added was the decision contract — an explicit verdict label,
kill criteria and a do-nothing comparison in every output (the generic
reviewer: none), and no veto headline for a cheap fix — and, after one
refactor, the data-contact rule: both arms had computed a protected outcome in
some reviews of an un-contacted pre-registration, and the refactored role did so
in none of its re-runs. The mentor gate was tested separately: without the
structural slot the mentor dispatched the audit but absorbed it (verdict shown
in none of three answers); with it, the verdict opened three of three. Small n,
one grader, same model family throughout.

**Consequences.** One audit costs about as much as a plain isolated review
(≈ +13 % in the source test). Claims about the role are limited to "adds the
decision contract and the data-contact rule"; never "catches more flaws".

**Alternatives rejected.**
- A generic "review this" dispatch — same catch rate in the test, but no
  verdict contract, and it broke the single-contact discipline.
- A shared-context devil's advocate inside the mentor — shares the proposer's
  assumptions, which is the failure this role exists to avoid.

```yaml
id: DECISION_006
title: Operator-facing documents lead with figures — SCIENTIFIC_RULES §9 (draw before you narrate) and research-mentor hard rule 8
date: 2026-10-12
proposer: joint:[fable-5.1, opus-5.5]   # planned by Fable 5.1, written by Opus 5.5 in the source project, 2026-10-11/12
decided_by: human        # accepted by the human operator 2026-10-12 (the operator chose "project skill + kernel rule" on 2026-10-11)
status: accepted
evidence: [core/SCIENTIFIC_RULES.md, core/roles/research-mentor.md, templates/research-kernel.overlay.template.md]
supersedes: ""
superseded_by: ""
```

**Context.** The operator reads, judges and signs the project's documents on a
reading surface (a page of cards). A week after asking for figures wherever
possible (「能畫圖就畫圖」), the operator found a new pre-registration card added
as several hundred lines of prose with no figure and no guide, and asked again
(「把他能用畫的就不要塞大量文字」). The cause: the drawing rule lived only in
two spec files inside a build directory and in a plan file; no role, memory or
check carried it, so the session that added the card never saw it. The operator
chose to make the rule durable in two places: a project skill that maintains the
reading surface, and one generic rule here.

**Decision (proposed).** SCIENTIFIC_RULES gains §9: an operator-facing document
leads with figures for flows, arm comparisons, decision rules, noise or power
statements and schedules; long normative text is folded, never deleted; a figure
accompanies its derivation and never replaces it; every label on a figure traces
to a source line; a fresh-context checker that authored none of it verifies
figures and glosses, and the author applies its text verbatim (a re-typeset math
gloss on a signable document also gets a math-review equivalence check).
`research-mentor` gains hard rule 8, which applies §9 to everything the mentor
puts in front of the operator. The overlay template gains a "Reading surface"
slot under research-mentor. Enforcement lives in each project (in the source
project, its page builder's check fails a card the operator must read or sign
that has no figure).

**Consequences.** RULES_HASH changes and every adapter is regenerated. A project
without a reading surface writes "none" in the slot; §9 then binds only the
documents handed to the operator. SCIENTIFIC_RULES' change policy asks for an
accepted entry: the operator accepts or rejects this one before the commit is
pushed.

**Alternatives rejected.**
- A new generic curator role for the reading surface — a many-file process for
  something project-specific by nature (each project's surface and builder
  differ).
- A project-only rule — it would not reach the other projects that use the kernel.

```yaml
id: DECISION_007
title: Add conductor — a shared-context pipeline role that dispatches every stage and does no substantive work; discipline-card format
date: 2026-10-30
proposer: joint:[fable-5.1, opus-5.5]   # planned by Fable 5.1 with the operator's plan-mode rulings, written by Opus 5.5, 2026-10-30
decided_by: human        # accepted by the human operator 2026-10-30 (in chat)
status: accepted
evidence: [core/roles/conductor.md, core/disciplines/README.md, core/roles/README.md, templates/research-kernel.overlay.template.md]
supersedes: ""
superseded_by: ""
```

**Context.** In the source project a manuscript pipeline runs about a dozen
stages per chapter (brief, draft, mechanical checks, math review, gate review,
fixes, operator sign-off, translation, publication), with the main session
re-typing the same check sequences at every stage and re-reading long agent
reports; the operator measured most of the session's spend as context re-read
and asked for review-quality material before any discussion, key points first.
`SCIENTIFIC_RULES` names "the orchestrator" in §1 and §3 without a role that
holds that duty. Surveyed projects (the academic-research-skills plugin's
pipeline orchestrator; research-superpowers; a public Claude Code research
workflow) separate orchestration from content and stop the operator only at
named checkpoints. The operator ruled (2026-10-30, plan mode): the role is
kernel-generic; the only mandatory stop in the source project is set in its
overlay; field packs get a format and an overlay slot only.

**Decision (proposed).** A fourteenth role, `conductor` (shared-context, model
`inherit`, effort session-default, no effort-policy entry). It does no
substantive work: each stage goes to the role, workflow or script that does it,
and a judgement turn in the session is an announced hat switch to that role,
which echoes its own ACK line (§3.4). It
asks for condensed returns, gives each agent one deliverable, moves recurring
mechanical work into scripts, hooks or workflow steps, and keeps the main
session the only committer, publisher and dispatcher. Checkpoints are MANDATORY
(overlay-named, never skipped or answered for the operator), FULL (after four
consecutive SLIM continues, a failed gate, an override, or before a deviation
from the pipeline of record) or SLIM (one line, default runs); FULL and
MANDATORY stops show a decision dashboard, wait for the operator's reply, and
carry a five-item self-check line (numbers, concession, trajectory, scope,
completeness). A SOFT-GATE may be overridden only on the operator's explicit
choice, recorded with its reason in the tracker; a HARD-STOP — including
anything `SCIENTIFIC_RULES` requires — is never overridden, only fixed and
re-run, or the gate changed by the route its layer allows (an accepted
`DECISIONS.md` entry for an overlay gate; the rules file's change policy for a
`SCIENTIFIC_RULES` gate). Dispatches carry only the inputs the dispatched role's
input contract allows (the anonymized bundle for `blind-reviewer`; the proposal
as written for `sparring-partner`). Verdicts pass through verbatim. Pipeline state lives in the project's
tracker file, never in memory. In a session wearing the role, the conductor is
the orchestrator of `SCIENTIFIC_RULES` §1 and §3. `core/disciplines/README.md`
fixes the format of discipline cards (evidence conventions, terminology,
reviewer questions, ethics and consent, the quantitative / qualitative / mixed
switch; every item sourced; model-written cards enter as proposed); no cards
are written yet.

**§9 reading.** The role treats a SLIM line or a chat dashboard as a digest that
points to documents, not as a §9 document the operator signs; anything published
to the reading surface or handed over as a file gets §9 in full. This entry
records that reading so it does not live only in a role spec; if the operator
reads §9 otherwise, §9 wins and the role's rule 12 is amended.

**Consequences.** `SCIENTIFIC_RULES.md` is unchanged (RULES_HASH stays
`0f0b1f5cddf7`); 34 adapters instead of 31. Each consuming project fills the
`### conductor` overlay slot (pipeline of record, MANDATORY checkpoints,
tracker, checks and SOFT gates, brief sources, discipline cards) before a
session wears the role. Unmeasured: the role has not yet run a full pipeline;
the source project pilots it on one chapter before any claim about savings.

**Alternatives rejected.**
- Keep orchestration inside `research-mentor` — the mentor is a discussion
  role; mixing dispatch, checkpoints and tracking into it is the drift the
  operator reported.
- An isolated orchestrator agent — it could not hold the operator's session or
  wait at the operator's checkpoints.
- A project-only orchestrator — the duty is named generically in
  `SCIENTIFIC_RULES` and recurs in every project with a multi-stage pipeline.

```yaml
id: DECISION_008
title: Plan-tier dispatch profile — the overlay declares the operator's plan, the conductor derives `guarded` or `open`; nested dispatch stays forbidden under both
date: 2026-11-02
proposer: fable   # Fable 5.1, from the operator's instruction in chat, 2026-11-02
decided_by: human        # accepted by the human operator 2026-11-02 (in chat)
status: accepted
evidence: [core/dispatch_policy.yaml, core/roles/conductor.md, templates/research-kernel.overlay.template.md, https://claude.com/pricing, https://support.claude.com/en/articles/17728952, https://code.claude.com/docs/en/sub-agents, https://code.claude.com/docs/en/statusline]
supersedes: ""
superseded_by: ""
```

**Context.** Under the rolling 5-hour usage window the operator ran one agent
at a time. Nothing wrote that down as a rule: the conductor's procedure
dispatches a stage and waits for its return (steps 2–6), the operator's
harness keeps workflows small, and the operator's own token rules make the
main session the only dispatcher, deny nested dispatch by a hook and bound
agent idle time. On 2026-11-02 the operator asked to lift the parallel cap for
plans that have no window — but only after the plugin asks which plan is in
use, and without touching the nesting ban. The vendor's public pages on that
date: Pro and Max meter a 5-hour window and a weekly limit; Ultra (the renamed
Max 20x) has no 5-hour window for verified accounts and keeps the weekly
limit; Team and Enterprise seats keep both windows; every plan keeps a weekly
limit; the plan tier is exposed neither to hooks nor to the model (the status
line sees only used percentages); the harness caps concurrent subagents at 20
and spawn depth at 3; subagent tokens count against the same limits.

**Decision (proposed).** A budget policy next to the effort policy:
`core/dispatch_policy.yaml` names two dispatch profiles. `guarded` (free,
pro, max, team-standard, team-premium): one isolated dispatch in flight at a
time from the main session; workflows small. `open` (ultra, enterprise, api):
independent stages in parallel from the main session, the number decided by
quality and the stages' dependencies only, never by speed, under the harness
ceiling; workflows unconstrained. The consuming project declares the
operator's plan tier once, in the overlay's `## Runtime budget` (with an
optional profile override); the conductor derives the profile, states it in
its first SLIM line or dashboard, and — when the slot is missing — asks the
operator once at session start, runs `guarded` meanwhile and offers the exact
line to paste, never editing the overlay itself (conductor hard rule 13).
Nothing else moves with the profile: the main session stays the only
dispatcher, committer and publisher; agents never dispatch; one deliverable
per agent and the idle limit hold; every dispatch carries the spec and the
hash; blind-review isolation is unchanged; parallel stages touch disjoint
files, get one tracker row each, and are read in dependency order. No role
requires a tier (SCIENTIFIC_RULES §8 "no paid-tier pins" still holds: the
policy adapts to whatever the operator has). "Open" never means free: every
plan keeps a weekly limit.

**Consequences.** `SCIENTIFIC_RULES.md` is unchanged (RULES_HASH stays
`0f0b1f5cddf7`). `tools/sync.py` reads the policy file and writes the mapping
into both adapters; the conductor spec and the overlay template carry the
slot; each consuming project adds one line. The operator's own harness
settings (workflow size, hooks) stay outside the kernel. Unmeasured: no
comparison of parallel against sequential spend under `open` exists yet; the
source project reports after one chapter run on `open`.

**Alternatives rejected.**
- Detect the plan automatically — the harness does not expose it; a guess
  that lands on `open` for a windowed plan is exactly the failure the cap
  prevented.
- Lift the nesting ban under `open` — a nested dispatcher must re-embed the
  spec and hash at every level and could commit or publish outside the main
  session; the operator declined it on 2026-11-02.
- Put the rule into `SCIENTIFIC_RULES.md` — a budget policy is not
  methodology, and the hash bump would stale every project adapter.
- Keep the tier in each project's state files — it is an operator fact, not
  a scientific record; the overlay is where operator facts already live.

```yaml
id: DECISION_009
title: Operations roles run Opus at xhigh — the operator's "Opus never below high" rule applied to experiment-runner and repo-maintainer
date: 2026-11-02
proposer: human   # the human operator, in chat 2026-11-02 ("Opus at least high" → "raise to xhigh"); entry drafted by Fable 5.1
decided_by: human        # accepted by the human operator 2026-11-02 (in chat)
status: accepted
evidence: [core/effort_policy.yaml, core/DECISIONS.md]
supersedes: ""   # amends only the effort line of DECISION_004; the model choice (opus) stands
superseded_by: ""
```

**Context.** DECISION_004 moved the two operations roles from a smaller model
at `low` to the strong model at `medium`: light reasoning, but a wrong launch
or a pushed protected path is expensive. On 2026-11-02, on an Ultra plan, the
operator set a standing rule for his own sessions that Opus never runs below
`high`, and when asked whether the plugin should follow, chose `xhigh` for
these two roles.

**Decision (accepted).** `experiment-runner` and `repo-maintainer` keep
`model: opus` and move to `effort: xhigh` in `core/effort_policy.yaml`. The
task-shape doctrine keeps its shape: closed verification at `xhigh`, operations
on the strong model now also at `xhigh`, everything else inherits the session;
no role pins `max`.

**Consequences.** Both wrappers are regenerated with `effort: xhigh`; the team
map's two badges and the roster doctrine sentence change; `SCIENTIFIC_RULES.md`
is unchanged (RULES_HASH stays `0f0b1f5cddf7`). Each operations dispatch costs
more tokens than under DECISION_004; the difference is unmeasured and the
source project reports it after a campaign.

**Alternatives rejected.**
- `high`, the minimum the operator's rule allows — the operator chose `xhigh`
  explicitly.
- Leave the plugin at `medium` and keep the rule personal — the operator's
  own sessions would then run the plugin's roles below his own floor.

```yaml
id: DECISION_010
title: No red-team role — the pilot found no detection gain over sparring-partner and seven false positives on sound work; the two residual triggers become a dispatch mode of sparring-partner
date: 2026-11-02
proposer: human   # the "black-faced bear" critic idea, in chat 2026-11-02 (provenance to be confirmed by the operator); the pilot design, PREREG.md and this entry drafted by Fable 5.1
decided_by: human        # accepted by the human operator 2026-11-02 (in chat: "DECISION_010 接受")
status: accepted
evidence: [eval/pilots/red-team-2026-11-02/PREREG.md, eval/pilots/red-team-2026-11-02/RESULT.md, eval/pilots/red-team-2026-11-02/reports/GRADES.md, eval/pilots/red-team-2026-11-02/RED_TEAM_DRAFT.md]
supersedes: ""
superseded_by: ""
```

**Context.** A fifteenth role was proposed: a dedicated adversarial critic
(`red-team`, the black-faced bear) that attacks a project's claim set and its
evidence before a reviewer does. An isolated `sparring-partner` audit of that
proposal (2026-11-02, in chat; gist recorded here because the audit text lives
only in the chat) returned `RETHINK`: the proposal's premises overlapped
`sparring-partner` (premise audit of a decision, evidence rule, read-only, never
a veto) and `blind-reviewer` (the manuscript) almost entirely; the residue that
neither covers is two triggers — *after a surprising result* and *on request
before submission* — and an output shape (ranked attacks, each with the cheapest
refuting test). The audit amended the draft before any pilot: isolated posture,
effort pinned at `xhigh`, no manuscript input, `sparring-partner`'s data-contact
rule; and it found that `AGENTS.md` had omitted `sparring-partner` from its
isolated-role list (a hand-typed list in `tools/sync.py`, now rendered from the
roster). The operator's rule was "pilot first, add only if it wins", then "run
the pilot now" on synthetic material rather than on a live project.

**Pilot (pre-registered in `PREREG.md`).** Five synthetic mini-projects, three
with one to three planted threats to validity and two sound controls, the key
sealed before any dispatch; ten dispatches in one batch — arm A
`sparring-partner` with the proposal "lock the claims as written, for
submission", arm B the amended draft contract — same model, same effort, same
four input files, same forbidden reads; reports blinded and graded against the
key by a fresh agent that ran no arm; the decision rule declared in advance:
add the role only if B finds strictly more planted threats (full + ½ partial)
than A **and** B's false positives on the controls are not greater than A's.
Result (`RESULT.md`): found A 7 = B 7, all seven graded full for both arms;
false positives on the controls A 0, B 7; a concrete refuting test named for
7 / 7 by both; arm B used about 1.6× the tokens and wrote about 3.5× the text.
Neither condition holds. Two deviations are recorded in `RESULT.md` §5 (the
material was written twice after a designer agent survived a session reset,
and the arms' one-line verdicts reached the orchestrator before grading);
neither touches a grade.

**Decision (accepted).** Verdict `mode`: no `red-team` role; the roster stays
at fourteen. `core/roles/sparring-partner.md` "When it is dispatched" gains the two residual triggers as a named dispatch mode — (a) after
a surprising result and (b) on request before submission — in both cases with
the proposal "lock the claims in `RESEARCH_STATE.md` as written" and the state
files as the evidence paths; `core/roles/README.md`'s roster line for
`sparring-partner` names the mode. The draft contract stays in the pilot
directory as a record and is not a role.

**Consequences.** Accepted 2026-11-02 and shipped in 0.5.1: the spec and the
roster line change (both READMEs' team tables name the mode), adapters are regenerated,
`SCIENTIFIC_RULES.md` stays untouched (RULES_HASH `0f0b1f5cddf7`). The pilot
directory (pre-registration, draft, briefs, bundles, raw and blind reports,
grades, result) is committed as the record; the key and the map stay sealed.
The operator's hand-drawn illustration set [corrected 2026-11-03: the set was generated by an image model at the operator's request, not hand-drawn] carries a fifteenth figure
(`critical-auditor`) with no role behind it; what to do with it is the
operator's call.

**Alternatives rejected.**
- Add the role for its output shape (ranked attacks with the cheapest refuting
  test) — the shape is reachable by wording the `sparring-partner` proposal,
  and the pilot shows the attack framing bought seven false positives on sound
  work and no extra detection.
- Run a larger pilot — the stopping rule was one run; a 7 = 7 tie on detection
  with a 7-vs-0 gap in false positives is not a close call that more bundles
  would flip.
- Fold "after a surprising result" into `research-mentor` — the mentor is
  shared-context and sees the proposer's reasoning; the trigger needs the
  isolation `sparring-partner` already has.

```yaml
id: DECISION_011
title: Rename the repository and plugin research-kernel → research-master; the research-os core name and the records keep their names
date: 2026-11-03
proposer: human   # the operator, in chat 2026-11-03: new name "research-master"; "改名，排在情境圖之前"
decided_by: human        # accepted by the human operator 2026-11-03 (in chat); the GitHub repository was renamed by the operator the same day
status: accepted
evidence: [CHANGELOG.md, .claude-plugin/plugin.json, .claude-plugin/marketplace.json, tools/sync.py, templates/research-master.overlay.template.md, README.md, README.zh-TW.md]
supersedes: ""
superseded_by: ""
```

**Context.** The operator asked for scenario figures that show how the roles
handle real situations (0.7.0). Every such figure carries the plugin's call
lines (`/<plugin>:<role>`), so the name had to be settled before any figure was
drawn. The operator chose `research-master` and renamed the GitHub repository
(GitHub shows it as `Research-Master`; repository names resolve
case-insensitively, so `brian10420/research-master` reaches it and the old
`research-kernel` URL redirects).

**Decision (accepted).** What changed in 0.6.0: the plugin and marketplace
names, homepage and repository URLs in `.claude-plugin/plugin.json` and
`marketplace.json`; the hardcoded name strings in `tools/sync.py` (the overlay
path `.claude/research-master.overlay.md`, the wrapper and adapter sentences,
the install commands); the overlay template, renamed
`templates/research-master.overlay.template.md`, and
`templates/project-state/README.md`; both READMEs (title, alt texts, install
commands, every `/research-master:<role>` call line, the overlay path,
`claude plugin details research-master`); the figures' kickers, install
commands and overlay path (`assets/*.svg`, `assets/roles/*.svg`) and the claims
tables that quote them in `assets/README.md`; the generated adapters and
wrappers, through `tools/sync.py`. What did not change: the provider-neutral
core's schema and doctrine name `research-os` (`schema: research-os/…` in every
state file and role spec) — it names the architecture, not the repository;
`core/SCIENTIFIC_RULES.md` and `core/roles/*.md`, which never contained the
repository name (RULES_HASH stays `0f0b1f5cddf7`); the records — `CHANGELOG.md`
history, earlier `DECISIONS.md` entries, `MIGRATION_AUDIT.md`, `eval/pilots/**`
— which keep the old name as written; the migration sentence in both READMEs,
which keeps the earlier rename and adds this one.

**Consequences.** A consumer re-adds the marketplace and reinstalls
(`/plugin marketplace add brian10420/research-master`, then
`/plugin install research-master@research-master`); the per-project overlay
file is renamed `.claude/research-master.overlay.md` (the wrappers look for the
new name only); GitHub redirects the old repository URL; the generated adapters
are regenerated, never hand-edited. No consumer exists yet except the operator's
own projects.

**Alternatives rejected.**
- Rename `research-os` too — it is the schema namespace in every state file of
  every consuming project; renaming it would touch per-project records for no
  gain.
- Rewrite the records to the new name — records are append-only; a rename is a
  new entry, not a silent edit.
- Keep the old name and draw the figures — the call lines in nine figures would
  have been wrong on the day they shipped.

```yaml
id: DECISION_012
title: No document-drafting role — paper-writer gains a document mode for outward documents (proposals, IRB protocols, consent forms, progress reports) under its hard rules, with rule 3 extended for the mode, plus document rails
date: 2026-11-03
proposer: human   # the operator's request in chat 2026-11-03 ("要，提 DECISION_012"); rails (a)–(f) are the operator's stated rules; rails (g)–(h) and this entry drafted by Fable 5.1; the sparring-partner audit's amendments (2026-11-03) folded in before the operator's decision
decided_by: human        # accepted by the human operator 2026-11-03 (in chat: re-test the tightened rail, then accept on all-PASS — "2", "通過再接受"; run 2 passed all four)
status: accepted
evidence: [core/roles/paper-writer.md, core/roles/README.md, core/roles/science-presenter.md, core/roles/results-analyst.md, core/HYPOTHESES.md, core/RESEARCH_STATE.md, core/disciplines/README.md, core/SCIENTIFIC_RULES.md, core/DECISIONS.md (DECISION_010), templates/research-master.overlay.template.md, eval/pilots/document-mode-2026-11-03/PREREG.md, eval/pilots/document-mode-2026-11-03/GRADES.md, eval/pilots/document-mode-2026-11-03/RESULT.md, eval/pilots/document-mode-2026-11-03/run2/PREREG.md, eval/pilots/document-mode-2026-11-03/run2/GRADES.md, eval/pilots/document-mode-2026-11-03/run2/RESULT.md]
supersedes: ""
superseded_by: ""
```

**Context.** The 0.7.0 scenario figures (a grant or collaboration proposal; an
IRB protocol and a consent form when a project crosses into a clinical study)
exposed a gap: no role may draft an outward document. `paper-writer` is scoped
to the project's own manuscripts; `science-presenter` makes audience-facing
materials, and its rule 4 says pre-launch work may be named, never numbered — a
proposal numbers unrun work by definition; a generic paper pipeline is
explicitly not a kernel role. A figure that drew a role drafting a proposal
would have stated something no spec states (SCIENTIFIC_RULES §9). The operator
asked for the capability as a decision, after a `sparring-partner` audit,
mirroring DECISION_010's pattern: a mode of an existing role, no new role.

**Decision (accepted).** `core/roles/paper-writer.md` gains a **Document mode**
(Purpose, Input and Output contract, and a "When it is used" list): outward
documents the project must produce — grant and collaboration proposals, IRB
protocols and consent forms, progress reports — drafted from the project's
state files (the design of record), under hard rules 1–7 — rule 3 extended for
the mode, the rest unchanged — plus document rails:

- (a) template sentences are copied verbatim — punctuation and alternatives
  such as "site(s)" included — and only the template's own fields (its
  bracketed or blank slots) change; a fact with no field of its own goes into
  the source note, never into a sentence; no section or table the template
  lacks (tightened after the dry-run below);
- (b) document names in plain words, never internal codes;
- (c) a person's title or position comes only from that person or the operator;
- (d) the first page states the whole case;
- (e) register and length follow the template (rail a) and, where the overlay
  (`## Disciplines`) names a discipline card, what that card says the field's
  reviewers expect (its "What reviewers in the field ask" and "Ethics and
  consent norms" sections); with no card, the template alone;
- (f) the operator reads every outward document in full before it leaves — the
  role never sends anything;
- (g) numbers in an outward document: a result claimed comes from a ledger row
  or `results-analyst` as in rule 3; a planned quantity (sample size, timeline,
  endpoints) comes from a `HYPOTHESES.md` entry with `status: accepted` and
  `decided_by: human` (its `test_plan` and `decision_rule` fields) or from the
  `RESEARCH_STATE.md` snapshot that points at those entries, and is always
  worded as a plan, never as a result — a `proposed` entry is never quotable;
  an administrative figure (budget, dates, personnel) is supplied by the
  operator and entered into the state file with `proposer: human` provenance
  before it enters the draft. No number without a named source; for an unrun
  study the design of record is the set of accepted hypotheses, not a locked
  story. Rule 3 is reworded to match: "Every number, table, and figure is
  sourced from `results-analyst` or a ledger row — in document mode, also from
  the sources rail (g) names";
- (h) SCIENTIFIC_RULES §9 lands on the operator's reading copy: the draft
  handed to the operator under rail (f) leads with figures wherever its content
  is a schedule, a comparison of arms or a power statement, and gets the
  fresh-context check; the submitted form follows its template (rail a).

A progress report spans every track: it may cite each track's claims in that
track's own approved phrasing; rule 5 forbids crossing claim sets between
manuscripts, not reporting on all of them.

Hand-offs: numbers → `results-analyst`; premises → `sparring-partner` (the
design phase of a large plan); field conventions → the discipline card;
committing → `repo-maintainer`. Boundary unchanged: the generic paper pipeline
is not this role. The overlay template gains a slot under `### paper-writer`:
"Outward-document templates and fill tool: <class → template path, tool>". The
roster line (`core/roles/README.md`) reads "discuss, revise, compile the
manuscript; document mode: proposals, protocols, consent forms, reports"; both
READMEs' team tables read "draft and revise the manuscript; document mode:
proposals, protocols, consent forms, reports".

**Audit (`sparring-partner`, isolated, 2026-11-03).** Verdict
`PROCEED-WITH-CHANGES`; the deciding premise was "hard rules 1–7 unchanged":
rule 3 admits a number only from `results-analyst` or a ledger row, so the
rails as first written could not state a sample size or a budget. Folded into
the text above: rule 3 extended for the mode; planned quantities sourced from
accepted, human-decided hypothesis entries instead of a new `## Planned design`
section (neither schema defines one, and the proposal gave it no provenance
block or acceptance rule) or "pre-registered" entries (`core/HYPOTHESES.md`
carries `status` and `decided_by`, not `pre_registered`; a model-proposed
entry would have qualified as the source of a sample size, against
SCIENTIFIC_RULES §2); rail (e) reworded because the card format carries no
register or length section; rail (h) added for §9; the progress-report
sentence and the roster verbs ("compile" kept) from the audit's minor items.
Verified by the audit: the gap is real (no spec names an outward document);
the RULES_HASH cannot move (`tools/sync.py` hashes only
`SCIENTIFIC_RULES.md`); the `### paper-writer` slot and `## Disciplines`
already reach the role through the wrapper; the `science-presenter` fold is
rightly rejected. The audit's kill criterion and cheapest test: one synthetic
dry-run before the spec edit, with two planted traps against a sealed expected
answer; the draft quoting the trap, supplying the absent figure, or adding a
sentence the template lacks would show that the rails as worded do not hold.

**Dry-run (2026-11-03, `eval/pilots/document-mode-2026-11-03/`).** Run at the
operator's request ("先跑演練後決定", in chat 2026-11-03) before any spec edit:
a synthetic project ("Harbor"), a partner's proposal template, two traps
planted and a key sealed before dispatch (`PREREG.md`; key sha256 prefix
`a11bc46af7a9c4f9`, gitignored); one dispatch of the proposed spec text as the
role's prompt; graded by a fresh agent that produced no draft (`GRADES.md`;
`RESULT.md`). Result: C1 (the model-proposed, un-accepted sample size) PASS —
the draft used the accepted design's figures and none of the proposed entry's;
C2 (the absent budget) PASS — the template's placeholders left byte-identical
and the budget listed as the first question for the operator; C4 (plan, not
result; unsourced phases blank) PASS; C3 (rail a) FAIL on one line of
thirty-two: the template's "recruited at [number] site(s)." became "recruited
at one site (Greywick Depot)." — the fixed fragment "site(s)." was resolved and
a fact without a field was added inside the sentence. Under the pre-registered
decision rule the rails as worded did not hold; the fix named there is to rail
(a)'s wording, not to the mode-vs-role choice. Rail (a) above is therefore
tightened to say what may change (the template's own fields) and where a fact
without a field goes (the source note). The tightened wording has not been
dry-run at that point (stopping rule: one dispatch, one grade). The
drafter's hand-off also behaved as rails (f) and (h) require: it sent nothing,
led its reading copy with two figures and their claims tables, and asked for
the fresh-context check it could not dispatch.

**Second dry-run (2026-11-03, `run2/`).** The operator chose to test the
tightened rail (a) before deciding, with acceptance pre-committed on all-PASS
("2", "通過再接受", in chat). Same material, same sealed key, one dispatch of
the spec as amended, a fresh grader that did not read the first run
(`run2/PREREG.md`, `run2/GRADES.md`, `run2/RESULT.md`). Result: C1, C2, C3 and
C4 all PASS — the diff against the template is nine slot lines, forty lines
byte-identical, "site(s)" kept verbatim, nothing added; both traps avoided;
every unsourced field blank and listed as a question. Two runs are not a
sample: the record supports that the tightened wording was operational on
this material, not that it holds everywhere.

**Process.** Written as `proposed`; audited by `sparring-partner` (isolated;
spec and hash in the prompt; the proposal as written and the evidence paths
above); amendments folded into this entry; two pre-registered dry-runs; the
operator accepted in chat; only then was the spec edited and `tools/sync.py`
run. `core/SCIENTIFIC_RULES.md` is untouched (RULES_HASH `0f0b1f5cddf7`).

**Consequences.** Accepted 2026-11-03 and shipped in 0.6.0:
`core/roles/paper-writer.md` gains the mode (the exact text dry-run in
`eval/pilots/document-mode-2026-11-03/PROPOSED_SPEC.md`) and rule 3's wording
changes as above; the roster line, both READMEs' team tables and the overlay
template's `### paper-writer` slot say so; adapters regenerated; `paper-writer` drafts outward documents from
the state files under the rails; a project that produces them fills the new
overlay slot and names its discipline card; figures 6 and 9 of 0.7.0 draw
`/paper-writer` in document mode. If rejected, the figures read "a dispatched
general-purpose agent, one deliverable — not a kernel role" at that step.

**Alternatives rejected.**
- A new `document-writer` role — DECISION_010's lesson: a role is added only
  when a pilot shows a gain over an existing role's mode; the drafting rules
  (claim ≤ evidence, never invent a number, never re-inflate) are
  `paper-writer`'s already; what differs is the input (a template and the
  design of record) and the rails.
- Fold it into `science-presenter` — its rule 4 forbids numbering pre-launch
  work and its output is for an audience; a proposal's numbers are plans and
  results with provenance, which is `paper-writer`'s discipline.
- A `## Planned design` section in `RESEARCH_STATE.md` as the source of planned
  quantities (the first draft of this entry) — the schema has no such section
  and the draft defined none; accepted hypothesis entries already carry
  `test_plan`, `decision_rule` and the §2 provenance block. The operator may
  reopen this.
- A dispatched general-purpose agent per document — the fallback if this entry
  is rejected; it loses the hard rules and the named hand-offs.
