---
schema: research-os/hypotheses/v1
scope: this project
seeded_from: research-os templates/project-state (schema v1)
---

# Hypotheses — Lumen

An idea is not evidence. A hypothesis moves to `accepted` when a human decides it
is worth testing; its *outcome* is written only from ledger rows.

## Entries

### H_001 — Lumen-Mix beats standard augmentation on Marigold-32 test top-1
- date: 2026-09-28 | proposer: human | status: accepted | decided_by: human
- statement: with ResNet-20 at the P1 budget, Lumen-Mix raises test top-1 by at least 0.8 points over flip + crop
- decision_rule: confirmed iff mean difference >= 0.8 and Welch p < 0.05 (two-sided); refuted otherwise
- test_plan: 3 seeds per arm under P1; about 18 GPU-hours
- linked_runs: [RUN_001, RUN_002, RUN_003, RUN_007, RUN_008, RUN_009, RUN_010]
- outcome: confirmed (+1.13, p = 0.006; RESEARCH_STATE C1)

### H_002 — Lumen-Mix beats Cutout on Marigold-32 test top-1
- date: 2026-09-28 | proposer: human | status: accepted | decided_by: human
- statement: Lumen-Mix raises test top-1 by at least 0.8 points over Cutout
- decision_rule: confirmed iff mean difference >= 0.8 and Welch p < 0.05; refuted iff mean difference < 0; inconclusive otherwise
- test_plan: 3 seeds per arm under P1 (Cutout rows shared with no other hypothesis)
- linked_runs: [RUN_004, RUN_005, RUN_006, RUN_007, RUN_009, RUN_010]
- outcome: inconclusive (+0.61 < 0.8; p = 0.049; RESEARCH_STATE C2 makes no claim)

### H_003 — label smoothing recovers the calibration lost by Lumen-Mix
- date: 2026-10-14 | proposer: fable | status: proposed | decided_by: ""
- statement: adding label smoothing 0.1 to Lumen-Mix brings test ECE below 0.025 without losing top-1
- decision_rule: not pre-registered; a single exploratory run only (RUN_011); no decision is taken from it
- test_plan: if pursued, 3 seeds under P1 with a pre-registered rule, after the deadline
- linked_runs: [RUN_011]
- outcome: "" (exploratory; RUN_011 single seed, ECE 0.019, top-1 92.40 — not a result)

### H_004 — the Lumen-Mix gain transfers to a wider backbone (WRN-16-4)
- date: 2026-10-20 | proposer: human | status: accepted | decided_by: human
- statement: with WRN-16-4 at the same budget, Lumen-Mix raises test top-1 by at least 0.8 over flip + crop
- decision_rule: same as H_001
- test_plan: RUN_012 (3 seeds) plus a WRN standard-aug arm (row to be opened at launch)
- linked_runs: [RUN_012]
- outcome: "" (not launched)
