# Comparison with existing SIS experience cards

## Purpose

This comparison explains why the VPN/V2rayNG/Hiddify extraction is being stored as a candidate package instead of being silently merged into `experience/sis/SIS_experience-cards.jsonl`.

The goal is to prevent duplicate-looking lessons while preserving the new client-layer incident evidence. Как обычно, главная угроза не в отсутствии файла, а в том, что следующий экземпляр прочитает половину и изобретёт остальное с видом пророка.

## Existing nearby lessons

Existing `SIS_experience-cards.jsonl` already contains reusable discipline relevant to this incident:

- recovery verification requires immutable external readback;
- push/publication is not proof without fresh readback;
- execution context and persistence must be verified;
- secret validation must not expose the secret;
- `systemd active` does not equal functional readiness;
- diagnostic clients must not guess JSON shape;
- stale semantic status after publication must be detected.

These are compatible with the new candidate cards, but they do not specifically cover the Android VPN client-layer distinction proven by Hiddify.

## What is new here

The VPN/Hiddify candidate adds a different class of reusable experience:

1. Alternative Android client as an early control test.
2. Same VLESS/Reality endpoint working in Hiddify while V2rayNG fails.
3. Controlled server upgrade treated as hypothesis closure when symptom does not change.
4. Correlated client/server capture as stronger evidence than timeout-only logs.
5. QR/URI bundle treated as secret operational material.
6. ARH placement acceptance separated from SIS operational acceptance.
7. Closed device/client registry requires SIS/KOO decision and secret-store model.

## Non-duplication assessment

This candidate overlaps with existing SIS discipline only at the abstract level:

- existing cards: generic verification, readback, secret handling, readiness and semantic status discipline;
- new candidate: VPN-specific client/server/provider localization and Android client replacement workflow.

Recommended treatment: keep as a candidate until SIS/KOO/ARH decide whether to merge into `SIS_experience-cards.jsonl`, create a SHD-specific cards file, or produce a cross-role runbook.

## Suggested merge targets

Possible future merge options:

1. Add all six cards to `experience/sis/SIS_experience-cards.jsonl` as `EXP-SIS-014..019` after SIS/KOO review.
2. Create `experience/shd/SHD_experience-cards.jsonl` if SHD receives official registration and its own experience lane.
3. Convert the extraction into a runbook: `experience/sis/android-vpn-client-diagnostics-runbook.md`.
4. Add one anti-regression test for future SIS/SHD instances: client timeout must trigger alternative-client control before production mutation.

## Evidence boundary

This comparison does not perform the merge. It only records that the candidate is non-duplicative enough to preserve.

---
КТО: SHD / ШАРДОВИК  
КОГДА: project_time omitted; trusted project-time source not used  
ДЛЯ ЧЕГО: сравнить VPN/Hiddify candidate cards с существующим SIS experience layer и не смешивать candidate с accepted material без review  
СТАТУС: comparison_candidate
