# SIS/SHD experience extraction candidate: VPN client layer and Hiddify resolution

## Purpose

This document extracts reusable experience from the completed Android VPN incident involving V2rayNG, Hiddify, Xray/VLESS/Reality, server-side diagnostics and public GitHub placement.

It is not a recovery snapshot, not a claim of current production state and not a canonical rule by itself. It is a candidate experience extraction for review and possible later merge into role-specific experience-current material.

Publication status: `candidate_from_confirmed_incident_report_and_arh_review`.

## Source boundary

Verified sources used:

- `puev5691/wellbeing-hq:entities/shardovik/outbox/SHD__vpn-v2rayng-hiddify-resolution__SIS.md`;
- `puev5691/wellbeing-hq:entities/archivarius/outbox/ARH__vpn-resolution-placement-review__SHD.md`;
- `puev5691/wellbeing-hq:entities/sisadmin/inbox/SHD__vpn-v2rayng-hiddify-resolution__SIS.md`;
- `puev5691/wellbeing-hq:registry/by-sender/shardovik.jsonl`;
- operator confirmation in the working chat that Hiddify works after import of existing profiles and V2rayNG gives the same failed result.

Excluded sources and data:

- full QR/URI bundle;
- UUID values;
- private keys, public key material where it could help recreate a working client profile, shortIds and access-sensitive parameters;
- raw server configuration;
- unverified current state of the production servers after the last confirmed episode.

## Entity scope

Primary beneficiary: `SIS` / СИСАДМИН.

Extractor/current executor: `SHD` / ШАРДОВИК.

Coordination dependencies:

- `KOO` decides whether and how to formalize SHD responsibilities and whether to require post-incident experience cards by default.
- `ARH` already accepted the redacted GitHub placement and secret boundary with bounded follow-ups.
- `SIS` remains the operational owner of infrastructure follow-up decisions.

## Incident summary

The Android VPN contour stopped working correctly through V2rayNG on mobile operators and Volna. Server-side checks showed that the Xray/VLESS/Reality chain and the WB-to-UK SOCKS/SSH route were functioning. A controlled Xray upgrade to `26.6.1` did not change the failed V2rayNG behavior. A new VLESS/Reality QR/client was generated and tested. V2rayNG still failed. The same profile path worked through Hiddify. Existing client profiles were then exported as QR/URI bundle and reinstalled on smartphones through Hiddify. Operator confirmed that everything works.

Practical outcome: `resolved_by_client_replacement`.

Causal boundary: this does not prove that V2rayNG is universally broken, and it does not prove that the server contour caused the original failure. It proves that for this project contour, at this stage, Hiddify works where V2rayNG did not.

## Extracted episodes

### 1. Server-first remediation was tempting but not decisive

Task: restore smartphone VPN connectivity.

Observation: the server chain was repeatedly checked, upgraded and tested, but the symptom did not change in V2rayNG. The decisive localization happened only when the same profile class worked in Hiddify.

Lesson: for Android VPN timeouts, do not start with server-side remediation as the default path if an alternative client can be tested quickly on the same endpoint/profile.

next_time_behavior: run an alternative-client control early, before production server changes, unless server-side evidence is already explicit and strong.

prohibited_repeat: do not interpret client timeout as server fault before comparing with another client on the same endpoint.

Applicability: Android proxy/VPN incidents with VLESS/Reality, Xray, TUN/socks clients and provider-dependent failures.

### 2. Same endpoint in a different client is a strong localization test

Task: distinguish server/protocol/provider failure from client implementation/profile handling failure.

Observation: V2rayNG failed on the problem networks, while Hiddify worked using the same project contour. This moved the working conclusion away from server failure and toward client-layer failure.

Lesson: a working alternative client on the same VLESS/Reality endpoint localizes the problem class to client handling, profile import, TUN, DNS, routing or local app behavior.

next_time_behavior: after server path sanity checks, test Hiddify/NekoBox or another independent client before changing transport or rotating infrastructure.

prohibited_repeat: do not keep changing server parameters after a different client already proves the endpoint can work.

Applicability: VLESS/Reality Android client diagnostics.

### 3. Correlated client/server capture is stronger than isolated timeout logs

Task: decide whether traffic reaches the server and whether the server path is alive.

Observation: client logs showed `context deadline exceeded` and TUN/socks errors. Server-side correlated captures showed incoming traffic and functioning local SOCKS/UK egress. Alone, neither side was enough; together they narrowed the problem.

Lesson: isolated client timeout is weak evidence. Correlating client behavior with server listener/connection/egress metrics produces a bounded and safer conclusion.

next_time_behavior: when possible, capture both client symptom and server-side ingress/egress during the same test window.

prohibited_repeat: do not draw a broad server/provider conclusion from only one timeout message.

Applicability: network incident diagnostics, especially when providers and client apps are both suspects.

### 4. Controlled upgrade must be treated as experiment, not cure

Task: remove possible Xray version mismatch between server and Android client core.

Observation: Xray was upgraded to `26.6.1` with preflight, backup, config-test, restart verification and rollback path. The symptom remained unchanged in V2rayNG.

Lesson: a controlled upgrade can close a compatibility hypothesis, but it is not evidence that upgrade was the cure unless behavior changes afterward.

next_time_behavior: state explicitly whether upgrade changed the symptom; if not, mark the hypothesis closed, not solved.

prohibited_repeat: do not narrate a successful upgrade as incident resolution when the operational symptom persists.

Applicability: production software upgrades during incident response.

### 5. QR/URI export is operationally useful but security-sensitive

Task: move existing clients to the working Android app.

Observation: existing VLESS/Reality clients were exported into QR/URI bundle and imported into Hiddify. This restored smartphone operation. Full QR/URI data was deliberately kept out of public GitHub.

Lesson: QR/URI bundles are access credentials and must be handled as secret artifacts outside public GitHub.

next_time_behavior: publish only redacted reports and secret-boundary facts; never publish full URI, UUID, private key, shortId or sensitive locator in public project field.

prohibited_repeat: do not include usable access material in reports, dispatch files, registry entries or public experience cards.

Applicability: VPN/proxy/client credential management.

### 6. Placement is not receipt, and receipt is not acceptance

Task: publish the result for SIS and ask ARH to check placement.

Observation: SHD created the report, inbox pointer, dispatch and sender registry record. ARH accepted placement with bounded follow-ups. SIS receipt and SIS acceptance remain separate absent events.

Lesson: GitHub placement, dispatch, receipt and semantic acceptance are separate states.

next_time_behavior: report status exactly: `dispatched`, `placement_accepted`, `received`, `accepted` only when corresponding evidence exists.

prohibited_repeat: do not call a published report accepted by SIS just because it is visible in SIS inbox or accepted by ARH for placement.

Applicability: all inter-entity GitHub information-field work.

### 7. Device/client registry is useful but must not be invented

Task: track which smartphone uses which client identity after migration.

Observation: ARH stated that a closed registry `device -> client identity/email -> installed client -> verification stage/result` would be operationally useful, but public GitHub is not the place for it and the registry does not yet exist by that review.

Lesson: closed operational registries are separate controlled artifacts; their existence, location and access model must be decided before use.

next_time_behavior: ask SIS/KOO for a decision on secret-store and minimal fields before creating or referencing a device/client registry.

prohibited_repeat: do not create a public locator for a secret registry and do not pretend a closed registry already exists.

Applicability: infrastructure inventory, credentials, device binding, client migration tracking.

## Candidate behavior tests

1. Android VPN failure with client timeout, but no explicit server failure: next entity must test an independent Android client before production server mutation.
2. Server upgrade succeeds but symptom persists: next entity must mark upgrade as hypothesis closure, not as resolution.
3. Alternative client works on same endpoint: next entity must stop server-first debugging and localize to client layer.
4. QR export needed: next entity must produce redacted public report and keep usable credentials out of GitHub.
5. Dispatch exists but receipt missing: next entity must keep status `dispatched`, not `accepted`.
6. ARH placement accepted but SIS has not replied: next entity must distinguish archival placement from operational acceptance.

## Recommended next routing

- SIS: decide whether to maintain Hiddify as default Android client until V2rayNG is retested or fixed.
- SIS/KOO: decide whether to create a closed device/client registry.
- KOO: decide whether post-incident experience extraction becomes mandatory for SHD/SIS incidents.
- ARH: no additional action required unless a new archive-route or secret-store policy is created.

## Status

`candidate_extraction_created`

---
КТО: SHD / ШАРДОВИК  
КОГДА: project_time omitted; trusted project-time source not used  
ДЛЯ ЧЕГО: извлечь переносимый опыт из завершённого VPN/V2rayNG/Hiddify incident для SIS/SHD и будущих anti-regression проверок  
СТАТУС: candidate_extraction_for_review
