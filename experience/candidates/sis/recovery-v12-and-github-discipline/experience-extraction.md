# SIS experience extraction candidate: recovery v1.2, network diagnostics, GitHub discipline

## Purpose

This is not a recovery snapshot and not a claim of current production state. It extracts reusable experience from the visible SIS working segment so the next SIS instance can avoid repeated mistakes and produce verified results faster.

Publication status: `candidate_operator_direct_no_koo_review`.

## Source boundary

Visible sources: current SIS chat segment; uploaded extraction protocol `KOO__OLD-CHAT-experience-extraction-task.md`; local SIS journal and recovery report files present in `/mnt/data`; GitHub readback from `puev5691/wellbeing-entity-bootstrap` and `puev5691/wellbeing-experience`; user-provided shell outputs visible in the current chat.

Not visible: full raw chat from the beginning; all old SIS chats; unprovided local files; live server state after the last checks.

## Entity

Entity: `SIS` / СИСАДМИН.

Actual work areas in this visible segment: server/network diagnostics, recovery v1.2, GitHub publication/readback, experience-layer comparison.

## Episodes

### Controlled tcpdump for mazhor to burzh

Task: determine whether TCP packets from `mazhor` reached `burzh` on ports `2222` and `443`.

Observation: a first tcpdump window without confirmed synchronous send was insufficient. A later controlled window aligned `mazhor` send time with `burzh` tcpdump time. The `mazhor` attempts failed and `burzh` did not show matching TCP packets.

Lesson: timeout is weak evidence until source, destination, filter and time window are aligned.

next_time_behavior: for inter-host diagnostics, verify host identity, start destination capture, run source send inside the capture window, then compare clocks before drawing a limited conclusion.

prohibited_repeat: do not infer route/firewall/provider cause from unsynchronised tcpdump or a lone timeout.

Applicability: reusable for manual TCP/UDP path diagnostics; not proof of provider-level filtering by itself.

### Invalid erefia to burzh check

Task: check `erefia -> burzh`.

Observation: commands labelled as `EREFIA SEND` were actually run from prompt `root@p552203`, i.e. `mazhor`; send also started after the capture window.

Lesson: command labels and human intent are not evidence. Prompt, hostname and time window are stronger.

next_time_behavior: before accepting a result, verify the actual host, actual direction and time overlap.

prohibited_repeat: do not record wrong-host output as evidence for the intended node.

Applicability: reusable for all multi-terminal manual operations.

### SIS recovery v1.2 assessment before creation

Task: create or migrate SIS recovery v1.2 only after checking existing materials.

Observation: before publication, `entities/sis` and expected recovery files returned `404`; searches did not reveal current SIS recovery; KOO registry/priority board named SIS as the next cycle and recovery not confirmed.

Working resolution: publish `SIS__initiation-current__SIS.md`, `SIS__snapshot__SIS.md`, `SIS__recovery-manifest__SIS.md`, `sha256sums.txt`; read them back; keep status `published_candidate_for_external_verification`, not `upgraded`.

Lesson: recovery creation requires conflict assessment and readback; status promotion belongs to KOO after independent verification.

next_time_behavior: check locator, expected files, repository search and KOO registry before creating current recovery candidate.

prohibited_repeat: do not create parallel current recovery blindly and do not self-assign `upgraded`.

Applicability: requires-current-check for entity recovery cycles.

### Scoped execution mode

Task: switch from one-step manual diagnostics to uninterrupted assessment/publish.

Observation: the Operator explicitly cancelled step pauses for the GitHub assessment, but did not authorize production DNS/firewall/Xray changes.

Lesson: cadence instruction changes stopping behavior, not safety boundaries.

next_time_behavior: batch safe file/GitHub read/search/create/readback, but request separate approval for destructive production changes.

prohibited_repeat: do not treat `без остановок` as permission to mutate production.

Applicability: reusable for workflow management.

### Experience layer as candidate, not current mutation

Task: publish these results to the project GitHub information field and compare with existing experience materials.

Observation: `puev5691/wellbeing-experience` already exists and contains corpus, ops, schemas, tests, views and entity experience cards. `ops/INGEST-ROUTE.md` sends extraction results through candidate/KOO review before promotion.

Working resolution: publish this extraction as candidate material and compare it with existing SIS cards instead of appending directly to `experience/sis/SIS_experience-cards.jsonl`.

Lesson: accumulation needs separation of raw, candidate, refined and current layers. Otherwise the system accumulates confident sludge and calls it wisdom, because apparently folders have to do morality now.

next_time_behavior: put fresh deltas in candidate paths, compare against current cards, then let KOO/dedupe promote selected lessons.

prohibited_repeat: do not overwrite or silently append to curated role cards without review.

Applicability: reusable for the experience repository.

## Rake plantation

1. Unsynchronised packet capture: tcpdump=0 without proven source send is inconclusive.
2. Wrong host masked by label: `root@p552203` beats `echo EREFIA SEND`.
3. Self-upgraded recovery: published candidate is not upgraded.
4. Cadence confusion: uninterrupted execution is not destructive authorization.
5. Current-card pollution: candidate deltas must not be merged into curated cards without review.

## Reusable procedures

### Controlled inter-host packet visibility check

Identify both hosts; start capture on destination; run send on source inside window; collect both outputs; align timestamps; write only the observed fact and limitations.

Stop on wrong host, missing send proof, missing capture output, ambiguous source IP or non-overlapping time windows.

### Recovery v1.2 assessment and candidate publication

Read task; check proposed locator; fetch expected files; search repository; read KOO registry/priority; classify current/legacy/unknown; create candidate files; compute checksums; publish; read back; report to KOO.

Stop on competing current recovery, inaccessible locator, checksum mismatch, secret requirement or production mutation requirement.

### Experience candidate publication and comparison

Find target repository; read corpus/route/current cards; prepare extraction/cards/tests; publish to candidate path; read back; compare with current layer; do not mutate curated role-specific cards without review.

## Historical unresolved state

open: correct `erefia -> burzh` packet visibility check.
open: DNS records for `wbnetrus.ru` neutral names.
open: KOO independent verification and cold-start of SIS recovery candidate.
parked: production DNS/firewall/Xray/SOCKS changes until separate task.
unknown: local or external SIS recovery outside checked GitHub paths.
unknown: live server state after the last visible checks.

## Extraction report

Viewed range: visible SIS segment around network diagnostics, recovery v1.2 publication and experience-layer comparison; not full raw chat.

Counts: episodes 5; rakes 5; causal decisions 5; reusable procedures 3; experience cards 5; anti-regression cases 5; major unknown categories 2.

---
created_by: SIS
created_when: generated_without_trusted_project_time
created_for: candidate extraction of visible SIS experience for GitHub experience-layer comparison