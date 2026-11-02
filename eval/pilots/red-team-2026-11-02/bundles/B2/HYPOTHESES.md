---
schema: research-os/hypotheses/v1
scope: this project
seeded_from: research-os templates/project-state (schema v1)
---

# Hypotheses — Sable

An idea is not evidence. A hypothesis moves to `accepted` when a human decides it
is worth testing; its *outcome* is written only from ledger rows.

## Entries

### H_001 — Sable beats BiLSTM-CRF on Corvid-NER test span-F1
- date: 2026-09-20 | proposer: human | status: accepted | decided_by: human
- statement: at the P1 budget, Sable raises test micro span-F1 by at least 1.0 point over BiLSTM-CRF
- decision_rule: confirmed iff mean difference >= 1.0 and Welch p < 0.05 (two-sided); refuted otherwise
- test_plan: 8-configuration sweep per arm on validation, then 3 seeds per arm under P1; about 30 GPU-hours
- linked_runs: [RUN_001, RUN_002, RUN_003, RUN_004, RUN_005, RUN_006, RUN_007, RUN_008, RUN_009, RUN_010, RUN_011, RUN_012, RUN_013, RUN_014, RUN_015]
- outcome: confirmed (margin met, p = 0.04; RESEARCH_STATE C1)

### H_002 — the boundary-aware term carries the Sable gain
- date: 2026-10-10 | proposer: human | status: accepted | decided_by: human
- statement: removing the boundary-aware term from Sable (S4) lowers test span-F1 by at least 1.0 point
- decision_rule: confirmed iff mean difference over 3 seeds >= 1.0 and Welch p < 0.05; refuted otherwise
- test_plan: 3 seeds of cfg/sable_noboundary.yaml under P1, compared with RUN_012, RUN_014, RUN_015
- linked_runs: [RUN_016, RUN_017, RUN_018]
- outcome: confirmed (−1.6; RESEARCH_STATE C2)

### H_003 — a character CNN adds to Sable on rare entity types
- date: 2026-10-16 | proposer: fable | status: proposed | decided_by: ""
- statement: adding a character CNN to the Sable encoder raises test span-F1 on EVENT and LAW by 2 points or more
- decision_rule: not pre-registered; a single exploratory run only (RUN_019); no decision is taken from it
- test_plan: if pursued, 3 seeds under P1 with a pre-registered rule, after the deadline
- linked_runs: [RUN_019]
- outcome: "" (exploratory; RUN_019 single seed, test F1 90.9 — not a result)

### H_004 — the Sable gain transfers to Heron-NER
- date: 2026-10-22 | proposer: human | status: accepted | decided_by: human
- statement: on Heron-NER at the same budget, Sable raises test span-F1 by at least 1.0 point over BiLSTM-CRF
- decision_rule: same as H_001
- test_plan: RUN_020 (3 seeds) plus a Heron-NER BiLSTM-CRF arm (row to be opened at launch)
- linked_runs: [RUN_020]
- outcome: "" (not launched)
