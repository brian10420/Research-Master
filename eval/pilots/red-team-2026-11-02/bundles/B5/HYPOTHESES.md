---
schema: research-os/hypotheses/v1
scope: this project
seeded_from: research-os templates/project-state (schema v1)
---

# Hypotheses — Kestrel

An idea is not evidence. A hypothesis moves to `accepted` when a human decides it
is worth testing; its *outcome* is written only from ledger rows.

## Entries

### H_001 — mined hard negatives raise test nDCG@10 over in-batch negatives
- date: 2026-09-23 | proposer: human | status: accepted | decided_by: human
- statement: at the P1 budget, Kestrel raises test nDCG@10 by at least 0.010 over the in-batch dual encoder
- decision_rule: confirmed iff mean difference >= 0.010 and Welch p < 0.05 (two-sided); refuted otherwise
- test_plan: 3 seeds per arm under P1; about 60 GPU-hours including index builds
- linked_runs: [RUN_001, RUN_002, RUN_003, RUN_004, RUN_005, RUN_006, RUN_007, RUN_008]
- outcome: confirmed (+0.013, p = 0.04; RESEARCH_STATE C1)

### H_002 — mined hard negatives raise test Recall@50 over in-batch negatives
- date: 2026-09-23 | proposer: human | status: accepted | decided_by: human
- statement: Kestrel raises test Recall@50 by at least 0.010 over the in-batch dual encoder
- decision_rule: confirmed iff mean difference >= 0.010 and Welch p < 0.05; refuted iff mean difference < 0; inconclusive otherwise
- test_plan: same rows as H_001 (secondary metric, no extra runs)
- linked_runs: [RUN_002, RUN_003, RUN_004, RUN_006, RUN_007, RUN_008]
- outcome: inconclusive (+0.007 < 0.010; RESEARCH_STATE C2 makes no claim)

### H_003 — the gain comes from BM25-mined negatives, not from the negative cache itself
- date: 2026-10-20 | proposer: human | status: accepted | decided_by: human
- statement: replacing the BM25-mined negatives with uniformly sampled negatives (same cache size, same budget) lowers Kestrel's test nDCG@10 by at least 0.010
- decision_rule: confirmed iff mean difference over 3 seeds >= 0.010 and Welch p < 0.05; refuted otherwise
- test_plan: RUN_010 (3 seeds of cfg/kestrel_randneg.yaml) compared with RUN_006, RUN_007, RUN_008; about 30 GPU-hours
- linked_runs: [RUN_010]
- outcome: "" (not launched; planned 2026-11-05)

### H_004 — a larger negative cache (32) adds to the Kestrel gain
- date: 2026-10-14 | proposer: fable | status: proposed | decided_by: ""
- statement: doubling the negative cache to 32 per query raises test nDCG@10 by 0.005 or more
- decision_rule: not pre-registered; a single exploratory run only (RUN_009); no decision is taken from it
- test_plan: if pursued, 3 seeds under P1 with a pre-registered rule, after the deadline
- linked_runs: [RUN_009]
- outcome: "" (exploratory; RUN_009 single seed, test nDCG@10 0.519 — not a result)
