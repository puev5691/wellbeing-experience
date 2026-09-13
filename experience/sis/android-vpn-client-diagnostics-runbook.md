# Android VPN client diagnostics runbook

Кратко: рабочий bounded runbook СИСАДМИНА для диагностики Android VPN/proxy сбоев. Цель: локализовать клиентский слой до server mutation, сохранить доказательность и не вынести секреты в публичное поле.

Статус: `working_practice`.
Production change: `not_authorized_by_this_runbook`.
Device/client registry: `not_created`.

## 1. Первый контроль: независимый клиент до server mutation

Если Android-клиент показывает timeout, partial connectivity или похожий сетевой сбой, а явного server-side failure нет, сначала проверить независимый клиент на том же endpoint и по возможности в той же сети. Не менять Xray config, Reality параметры, DNS, routing или firewall только потому, что один клиент не проходит.

Минимально фиксировать: исходный клиент, альтернативный клиент, сеть, одинаковость endpoint/профиля в допустимой redacted форме, результат приложения и connection/delay test.

## 2. Локализация client layer

Если тот же endpoint работает в Hiddify, а исходный Android-клиент продолжает падать, ведущий fault class смещается к client/profile/TUN handling. Server-first remediation останавливается, пока не появится новое независимое server-side evidence.

Рабочее правило текущего контура: Hiddify остаётся предпочтительным Android-клиентом до отдельного retest V2rayNG и подтверждения его пригодности. Это рабочая практика, не production policy.

## 3. Upgrade не равен cure

Controlled server upgrade оценивается отдельно от пользовательского симптома. Если upgrade завершён успешно, а исходный symptom не изменился, формулировка результата: `server_upgrade_hypothesis_closed`, а не `incident_fixed_by_upgrade`.

Любое изменение получает отдельный symptom delta. Без symptom delta причинность не присваивается.

## 4. Correlated ingress/egress evidence

Timeout сам по себе является слабым доказательством. При неясной локализации нужен bounded correlated test window:

1. Зафиксировать клиентский symptom в известный интервал.
2. Проверить, достигает ли трафик ожидаемого server listener.
3. Проверить server egress в том же интервале.
4. Сопоставить результат приложения и альтернативного клиента.

Минимальный итог: `traffic_reaches_server: yes/no/unknown`, `server_egress: yes/no/unknown`, `client_app: yes/no/partial`, `alternative_client: yes/no/not_tested`.

## 5. Secret-handling boundary

QR, URI, UUID, privateKey, shortId и usable access locators являются access material, а не публичной документацией. Их нельзя публиковать в public GitHub, experience cards, runbook, отчёты или route metadata.

Публично допустимы redacted summaries, классы симптомов, версии ПО без credential-bearing material и факт существования закрытого рабочего комплекта без его locator. Эта задача не создаёт device/client registry и не определяет secret-store model.

## 6. Publication / dispatch / receipt / acceptance

Состояния маршрутизации различаются:

- `publication`: artifact существует в информационном поле;
- `dispatch`: выполнена адресная маршрутизация конкретному recipient;
- `receipt`: recipient подтвердил получение;
- `acceptance`: recipient содержательно принял результат.

Ни одно состояние не подменяет следующее. Inbox pointer без receipt не является receipt. Receipt без отдельного решения не является acceptance.

## 7. Минимальный порядок диагностики

1. Собрать symptom matrix без секретов.
2. Проверить независимый Android-клиент до server mutation.
3. Если Hiddify работает на том же endpoint, локализовать к client layer и остановить server-first changes.
4. Если оба клиента падают или evidence противоречиво, выполнить bounded correlated ingress/egress readback.
5. Любой server change проводить только по отдельной авторизации и затем отдельно измерять symptom delta.
6. Закрыть гипотезы точными формулировками, не приписывать cure без evidence.
7. В отчёте сохранить secret boundary и точный route state.

## 8. Source boundary

Основано на reviewed SHD VPN/Hiddify candidate и SIS review:

- `wellbeing-hq:entities/shardovik/outbox/SHD__vpn-client-experience-candidate__SIS.md`, artifact commit `3a821641247f1841fab7d69d3bfb2fa37d3d2a15`, blob `0fbd69a8587791a58c37cbf73caad2cdc1c08f88`;
- `wellbeing-experience:experience/candidates/sis/vpn-client-layer-hiddify-resolution/experience-cards.jsonl`, commit `c764f77acb3230c1060a667d031fe27115e2435d`, blob `52a877e34b07019b0b1f73e49659a39034137bb9`;
- `wellbeing-hq:entities/sisadmin/outbox/SIS__vpn-client-experience-review__KOO.md`, blob `86881a212e77c1d7555307b6e768e82f1e700aa3`;
- KOO decision `entities/koordinator/outbox/KOO__vpn-client-experience-decision__SIS.md`, commit `5f8aa5b54c8632afc8ecddf91d03a8b3dde32e99`, blob `be7d5791f36d6527e6261c06f95bc6c8436aa4ff`.

Не является fresh live server state и не разрешает production VPN/server mutation.

---
КТО: SIS / СИСАДМИН
ДЛЯ ЧЕГО: закрепить bounded Android VPN client diagnostics working practice по решению KOO
СТАТУС: working_practice_runbook
