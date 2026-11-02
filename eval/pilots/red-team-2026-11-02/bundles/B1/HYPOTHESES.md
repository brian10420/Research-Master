---
schema: research-os/hypotheses/v1
scope: this project
seeded_from: research-os templates/project-state (schema v1)
---

# Hypotheses — Quarry

An idea is not evidence. A hypothesis moves to `accepted` when a human decides it
is worth testing; its *outcome* is written only from ledger rows.

## Entries

### H_001 — Quarry-Net beats tuned GBT on Kiln-Yield test RMSE
- date: 2026-09-22 | proposer: human | status: accepted | decided_by: human
- statement: at the P1 budget, Quarry-Net lowers test RMSE (standardised target) by at least 0.010 against the tuned GBT
- decision_rule: confirmed iff mean difference <= −0.010 and Welch p < 0.05 (two-sided); refuted otherwise
- test_plan: 4 seeds per arm under P1; about 26 GPU-hours (Quarry-Net) + 9 CPU-hours (GBT)
- linked_runs: [RUN_001, RUN_002, RUN_003, RUN_004, RUN_005, RUN_006, RUN_007, RUN_008, RUN_009, RUN_010]
- outcome: confirmed (−0.019, p < 0.001; RESEARCH_STATE C1)

### H_002 — the Quarry-Net gain holds on a second plant (plant B)
- date: 2026-10-18 | proposer: human | status: accepted | decided_by: human
- statement: on plant B's feature set, Quarry-Net lowers test RMSE by at least 0.010 against a GBT tuned with the same budget
- decision_rule: same as H_001
- test_plan: RUN_012 (4 seeds) plus a plant-B GBT arm (row to be opened at launch)
- linked_runs: [RUN_012]
- outcome: "" (not launched)

### H_003 — a log-transformed target reduces Quarry-Net error on the high-yield tail
- date: 2026-10-13 | proposer: fable | status: proposed | decided_by: ""
- statement: training on log(yield) and inverting at prediction lowers test RMSE by 0.005 or more without raising MAE
- decision_rule: not pre-registered; a single exploratory run only (RUN_011); no decision is taken from it
- test_plan: if pursued, 4 seeds under P1 with a pre-registered rule, after the deadline
- linked_runs: [RUN_011]
- outcome: "" (exploratory; RUN_011 single seed, test MSE 0.170 — not a result)
