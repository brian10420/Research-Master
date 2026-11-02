---
schema: research-os/research-state/v1
scope: this project
seeded_from: research-os templates/project-state (schema v1)
---

# Research State — Sable (span tagger with a boundary-aware loss, sequence labelling)

One snapshot per update, newest first. Numbers quoted here cite their ledger row
or the committed source of record (`scripts/report_p1.py` at code_rev `9b4d0f3`).

## Snapshots

```yaml
state_as_of: 2026-10-29
updated_by: human
status: accepted
sources: [RUN_001-RUN_020, H_001-H_004, PROTOCOL.md (P1), DEC_002]
active_tracks:
  - sable: P1 comparison and the boundary-term ablation complete; claims below proposed for lock
  - second-dataset: RUN_020 planned (H_004); not part of the claim set
frozen:
  - P1 protocol (split, seeds, epochs, checkpoint rule, tuning budget) frozen 2026-09-24 (DEC_002)
  - claim numbers frozen 2026-10-29 from scripts/report_p1.py at 9b4d0f3
blocked: []
deadlines: submission 2026-11-21
next_actions:
  - paper-writer: draft the results section from this file and the ledger only
  - experiment-runner: launch RUN_020 after the deadline (not for the paper)
guards:
  - "91.3" is the Sable test span-F1 headline (protocol P1, RUN_012, RUN_014, RUN_015)
  - "n = 3 seeds per arm" appears in every table caption
```

## Current claims (proposed for submission lock)

1. **C1 (H_001, confirmed).** On Corvid-NER test, Sable reaches micro span-F1 91.3
   against 88.5 for BiLSTM-CRF (+2.8 points; protocol P1, 3 seeds per arm; RUN_012,
   RUN_014, RUN_015 vs RUN_001–RUN_003). The pre-registered margin (1.0) is met and
   the Welch t-test gives p = 0.04 (df ≈ 3.0).
2. **C2 (H_002, confirmed).** Removing the boundary-aware term costs 1.6 F1 on average
   (88.8 against 90.4 over three seeds; RUN_016–RUN_018 vs RUN_012, RUN_014, RUN_015),
   so the term, not the span architecture alone, carries the gain.
3. **C3 (design).** Both arms received the same tuning budget (PROTOCOL §7: an
   8-configuration random search on the validation split, then 3 seeds of the best
   configuration), so the C1 gain is not a tuning artefact.
4. **C4 (scope).** All results are on one dataset (Corvid-NER v3, 9 flat entity types)
   at a fixed 40-epoch budget; RUN_019 (character CNN) is exploratory and excluded.
   No claim is made about other datasets or about nested entities.

## Result summary (test split, n = 3 seeds per arm; per-seed values in the ledger)

| arm | runs | span-F1 (mean ± sd) | val span-F1 (mean) |
| --- | --- | --- | --- |
| BiLSTM-CRF | RUN_001–RUN_003 | 88.5 ± 0.4 | 89.2 |
| Sable | RUN_012, RUN_014, RUN_015 | 91.3 ± 0.8 | 91.1 |
| Sable − boundary term | RUN_016–RUN_018 | 88.8 | 89.5 |

Source of record: `scripts/report_p1.py` at `9b4d0f3`; it reads the ledger rows above.

## Known limitations (stated in the paper)

- n = 3 seeds per arm (budget); the intervals above are sample sd over 3 values.
- RUN_013 (Sable, seed 2) failed at epoch 23 when the checkpoint write hit the disk
  quota and was relaunched as RUN_014 with an identical config; the failed row stays.
- RUN_019 (Sable + character CNN) is exploratory and not pre-registered; it is
  excluded from every claim and appears only in the ledger and H_003.
- Per-type F1 is reported in the appendix as found; two rare types (EVENT, LAW) have
  under 40 test spans and their per-type numbers are not interpreted.
