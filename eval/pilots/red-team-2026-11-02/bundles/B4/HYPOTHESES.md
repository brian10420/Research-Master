---
schema: research-os/hypotheses/v1
scope: this project
seeded_from: research-os templates/project-state (schema v1)
---

# Hypotheses — Tidewater

An idea is not evidence. A hypothesis moves to `accepted` when a human decides it
is worth testing; its *outcome* is written only from ledger rows.

## Entries

### H_001 — Tidewater beats seasonal-naive on Harbor-Load 24-h MASE
- date: 2026-09-25 | proposer: human | status: accepted | decided_by: human
- statement: over the four P1 rolling origins, Tidewater's fold-paired MASE difference against seasonal-naive is at most −0.10
- decision_rule: confirmed iff mean fold-paired difference <= −0.10 and paired t-test p < 0.05 (n = 4 folds); refuted iff mean difference > 0; inconclusive otherwise
- test_plan: 4 folds x 3 seeds for Tidewater; naive is deterministic; about 10 GPU-hours
- linked_runs: [RUN_001, RUN_002, RUN_003, RUN_004, RUN_017, RUN_018, RUN_019, RUN_020, RUN_021, RUN_022, RUN_023, RUN_024, RUN_025, RUN_026, RUN_027, RUN_028, RUN_029]
- outcome: confirmed (−0.263, p < 0.001; RESEARCH_STATE C1)

### H_002 — Tidewater beats tuned GBT on Harbor-Load 24-h MASE
- date: 2026-09-25 | proposer: human | status: accepted | decided_by: human
- statement: over the four P1 rolling origins, Tidewater's fold-paired MASE difference against the tuned GBT is at most −0.03
- decision_rule: confirmed iff mean fold-paired difference <= −0.03 and paired t-test p < 0.05 (n = 4 folds); refuted iff mean difference > 0; inconclusive otherwise
- test_plan: 4 folds x 3 seeds per arm; about 10 GPU-hours + 6 CPU-hours
- linked_runs: [RUN_005, RUN_006, RUN_007, RUN_008, RUN_009, RUN_010, RUN_011, RUN_012, RUN_013, RUN_014, RUN_015, RUN_016, RUN_017, RUN_018, RUN_019, RUN_020, RUN_021, RUN_022, RUN_023, RUN_024, RUN_025, RUN_026, RUN_027, RUN_028, RUN_029]
- outcome: inconclusive (−0.016, 95% CI −0.039 to +0.007, p = 0.11; RESEARCH_STATE C2 makes no claim)

### H_003 — a holiday-calendar feature removes the fold-3 degradation
- date: 2026-10-15 | proposer: fable | status: proposed | decided_by: ""
- statement: adding a holiday-calendar indicator to Tidewater lowers fold-3 test MASE below 0.82
- decision_rule: not pre-registered; a single exploratory run only (RUN_030); no decision is taken from it
- test_plan: if pursued, all 4 folds x 3 seeds for both learned arms with a pre-registered rule, after the deadline
- linked_runs: [RUN_030]
- outcome: "" (exploratory; RUN_030 single seed, single fold, test MASE 0.801 — not a result)

### H_004 — the Tidewater result transfers to region B
- date: 2026-10-21 | proposer: human | status: accepted | decided_by: human
- statement: on region B's 9 sites, Tidewater's fold-paired MASE difference against seasonal-naive is at most −0.10
- decision_rule: same as H_001
- test_plan: RUN_032 (4 folds x 3 seeds) plus a region-B naive arm (rows to be opened at launch)
- linked_runs: [RUN_032]
- outcome: "" (not launched)
