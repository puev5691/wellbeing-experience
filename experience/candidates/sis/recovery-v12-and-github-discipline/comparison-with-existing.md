# SIS experience comparison with existing wellbeing-experience layer

## Purpose

This report compares the fresh SIS candidate extraction with the existing GitHub experience field. It does not promote the candidate into curated role-specific cards.

## Checked GitHub field

Target repository found: `puev5691/wellbeing-experience`.

Repository README describes it as the accumulated experience base for project entities and the Continuity v2 technology contour. Its stated purpose is to store verified experience that changes behavior of future entity instances, not chat biographies and not recovery snapshots.

Repository layout checked: root files `README.md`, `CORPUS-MODEL.md`, `MANIFEST.md`, `SHA256SUMS.txt`; directories `experience/`, `ops/`, `raw/`, `registry/`, `schemas/`, `tests/`, `views/`.

Existing `experience/` entity directories: `kod`, `sis`, `vol`.

Existing SIS file checked: `experience/sis/SIS_experience-cards.jsonl`; it contains 13 cards, `EXP-SIS-001` through `EXP-SIS-013`.

## Comparison

| New candidate card | Overlap with existing SIS layer | Added value |
|---|---|---|
| `EXP-SIS-LIVE-014` controlled tcpdump | Reinforces existing readback/evidence discipline | Adds source/destination/filter/time alignment for packet visibility diagnostics |
| `EXP-SIS-LIVE-015` wrong host/time window | Extends `EXP-SIS-009` process/service identity lesson | Adds prompt/hostname/time-window validation for manual multi-host checks |
| `EXP-SIS-LIVE-016` recovery v1.2 assessment | Overlaps `EXP-SIS-001`, `EXP-SIS-005`, `EXP-SIS-013` | Adds current v1.2 cycle: conflict assessment before creation, KOO registry evidence, no self-upgraded status |
| `EXP-SIS-LIVE-017` scoped uninterrupted mode | Related to safety boundaries in operational cards | Adds distinction between execution cadence and authorization boundary |
| `EXP-SIS-LIVE-018` candidate experience publication | Matches repository design docs rather than old SIS cards | Adds ingestion invariant: fresh deltas go to candidate path, not curated current card mutation |

## Architectural answer

Yes, accumulation is happening, but not as automatic self-learning yet.

What exists:

- state continuity: recovery v1.2 packages in `wellbeing-entity-bootstrap`;
- experience continuity: `wellbeing-experience` with corpus model, ops route, schemas and entity cards;
- behavioral verification: anti-regression cases and tests directories.

What is not yet proven:

- automatic promotion from candidate to curated cards;
- regular dedupe/contradiction review;
- automatic construction of role-specific current views;
- mandatory behavioral admission tests before a new entity instance starts work;
- model-weight learning or autonomous self-modification.

Conclusion: this is an external, auditable experience-continuity architecture. It can support a self-correcting entity process if the candidate-to-current pipeline is enforced. It is not yet a self-learning entity in the strong technical sense. Currently it is a disciplined memory system, which is already a shocking improvement over the traditional human method: forget everything, improvise, and call it management.

## Recommended promotion route

Do not append this candidate directly to `experience/sis/SIS_experience-cards.jsonl`.

Recommended next handling by KOO:

1. review candidate source boundary and secret safety;
2. register candidate in `registry/INTAKE.jsonl`;
3. dedupe against `EXP-SIS-001` through `EXP-SIS-013`;
4. promote selected cards into curated SIS layer;
5. promote anti-regression cases into tests;
6. build a `views/sis` current view suitable for cold-start loading.

---
created_by: SIS
created_when: generated_without_trusted_project_time
created_for: comparison of fresh SIS candidate extraction with existing wellbeing-experience layer