# Android VPN client diagnostics runbook

Кратко: этот runbook описывает порядок диагностики Android VPN/proxy сбоев в контуре Xray/VLESS/Reality, когда пользователь видит timeout, приложения частично открываются или один клиент не работает, а другой может работать на том же серверном контуре.

Статус: `candidate_from_vpn_hiddify_resolution_experience`.

Этот документ не является project canon, не является текущим состоянием серверов и не содержит QR/URI/UUID/privateKey/shortId. Он предназначен для СИСАДМИНА как рабочий кандидат процедуры.

## 1. Назначение

Runbook нужен, чтобы следующий СИСАДМИН или SHD-исполнитель не начинал с изменения production-сервера при первом Android timeout. В завершённом инциденте V2rayNG на Android продолжал давать сбой, а Hiddify на том же VLESS/Reality контуре работал. Это локализовало проблему в клиентском слое и показало, что server-first remediation не должен быть реакцией по умолчанию.

Главная формула:

```text
симптом клиента → контроль альтернативным клиентом → read-only server/client correlation → только потом bounded remediation
```

## 2. Область применения

Применять, когда есть один или несколько признаков:

- Android-клиент VPN/proxy показывает timeout, context deadline exceeded или похожий сетевой сбой;
- Telegram, YouTube, браузер или другие приложения открываются частично;
- сбой зависит от сети: сотовый оператор, Wi-Fi провайдера, локальный Wi-Fi;
- серверная цепочка может быть исправна, но пользовательские приложения не работают;
- используется Xray/VLESS/Reality, TUN, socks/http proxy, SSH SOCKS, split-routing, DNS или QUIC/UDP 443.

Не применять как замену production-runbook для планового обновления сервера. Этот документ про диагностику клиентского слоя и границ причинности.

## 3. Запрещённые короткие пути

До независимой проверки нельзя:

- объявлять сервер причиной только по Android timeout;
- менять Xray config, Reality keys, routing, DNS или firewall вслепую;
- публиковать QR/URI/UUID/privateKey/shortId в публичный GitHub;
- считать успешное обновление сервера решением, если пользовательский симптом не изменился;
- считать GitHub publication равной delivery, receipt или acceptance;
- создавать публичный device/client registry без решения SIS/KOO и secret-store model.

Да, скучно. Зато потом не приходится выкапывать инфраструктуру из ямы, которую сами же радостно выкопали.

## 4. Минимальный сбор симптомов от ОПЕРАТОРА

Нужно получить не эссе о страданиях приложений, а короткую матрицу:

```text
Устройство: __
Клиент: V2rayNG / Hiddify / NekoBox / другое
Версия клиента: __
Сеть: mobile operator / Волна / Севстар / другое
Режим: VPN/TUN / proxy-only / unknown
Telegram: работает / connecting / не открывается / частично
YouTube: работает / только интерфейс / не открывается / частично
Delay/test result: число / timeout / context deadline exceeded / другое
Лог клиента: приложен / не приложен
```

Если есть несколько сетей, проверять одну и ту же пару `клиент + профиль` на каждой сети, не меняя сразу всё подряд. Люди любят менять пять переменных и потом делать вид, что эксперимент был научным. Не надо так.

## 5. Первый контроль: альтернативный клиент

Если текущий клиент V2rayNG даёт timeout, ранний контроль должен быть таким:

1. Импортировать тот же профиль или новый redacted-approved profile в Hiddify либо другой независимый Android-клиент.
2. Проверить Telegram.
3. Проверить YouTube.
4. Проверить встроенный connection/delay test.
5. Зафиксировать результат по той же сети.

Интерпретация:

```text
V2rayNG fails + Hiddify works = основной подозреваемый клиентский слой V2rayNG/profile/TUN handling.
Both clients fail = переходить к server/provider/profile correlation.
Both clients work = старый профиль/локальное состояние клиента было деградировано, нужен cleanup.
```

Рабочий альтернативный клиент на том же endpoint не доказывает, что V2rayNG исправлен. Он доказывает, что серверный контур в принципе способен работать.

## 6. Клиентские признаки, которые нужно искать в логах

Полезные строки и классы событий:

- core started / Xray started;
- accepted tcp/udp from TUN or socks;
- `context deadline exceeded`;
- `connection was refused` в proxy/tun;
- DNS-запросы к 1.1.1.1, 8.8.8.8 или системному resolver;
- `udp:...:443 -> block` или похожие признаки QUIC/UDP 443;
- явные Reality/TLS/certificate/fingerprint/publicKey/shortId ошибки, если они есть.

Если core стартует и трафик маршрутизируется, это не то же самое, что успешное приложение. Нужно сравнить с server-side evidence.

## 7. Read-only server-side checks

До production mutation разрешены только read-only проверки:

```text
systemctl status xray
xray -test -config /path/to/config.json
ss -ltnup | grep expected ports
journalctl bounded window, если доступно
curl/wget через локальный SOCKS, если схема его использует
проверка egress к целевым доменам без раскрытия секретов
```

Нужно отдельно фиксировать:

- Xray active/running не равен working end-user path;
- config test OK не равен доказательству, что клиентский профиль корректно обработан;
- server egress OK не равен доказательству, что Android-клиент правильно туннелирует приложения.

## 8. Correlated client/server capture

Если альтернативный клиент не прояснил ситуацию или оба клиента падают, нужен связанный тест:

1. На сервере начать bounded capture/metrics window.
2. На Android в это окно включить профиль.
3. Открыть Telegram.
4. Открыть YouTube.
5. Запустить client delay/test.
6. Остановить capture.
7. Сравнить ingress на server listener, локальный outbound/SOCKS, upstream path и клиентский симптом.

Результат фиксировать ограниченно:

```text
traffic reaches server: yes/no/unknown
server egress works: yes/no/unknown
client app works: yes/no/partial
alternative client works: yes/no/not-tested
```

Timeout без такой корреляции слаб. Timeout с доказанным server ingress/egress уже указывает на другой класс проблем.

## 9. Production update rules

Обновление сервера допустимо только как controlled remediation/hypothesis closure:

- есть preflight;
- известен origin установки;
- есть backup;
- есть config test before;
- есть exact target version;
- есть rollback path;
- есть config test after;
- есть service/liveness check;
- есть symptom delta после обновления.

Если upgrade succeeded, но Android symptom unchanged, статус:

```text
upgrade_successful_hypothesis_closed
```

А не:

```text
incident_fixed_by_upgrade
```

Иначе следующий исполнитель решит, что надо снова крутить сервер, потому что текст заманчиво соврал.

## 10. QR/URI handling

QR/URI bundle является access material, а не обычной документацией.

Правила:

- не публиковать полный VLESS URI;
- не публиковать QR-картинки;
- не публиковать UUID, privateKey, shortId и чувствительные locators;
- публично разрешены только redacted summaries и факт существования закрытого рабочего комплекта;
- закрытый device/client registry создавать только после SIS/KOO decision.

Если нужен перенос клиентов на Hiddify, создавать отдельный secret bundle outside public GitHub и публичный redacted report.

## 11. Итоговые статусы

Использовать точные статусы:

```text
resolved_by_client_replacement
server_upgrade_hypothesis_closed
client_layer_suspected
provider_path_unproven
secret_bundle_outside_public_github
placement_accepted
receipt_missing
acceptance_missing
```

Не смешивать:

- publication;
- dispatch;
- receipt;
- acceptance;
- archival placement review;
- operational acceptance by SIS.

## 12. Минимальный отчёт после инцидента

Итоговый report должен содержать:

1. Symptom matrix.
2. Проверенные клиенты и сети.
3. Что доказал альтернативный клиент.
4. Что доказали server-side checks.
5. Что изменялось в production, если изменялось.
6. Symptom delta после каждого изменения.
7. Secret boundary.
8. Open follow-ups.
9. Адресата и route status.

## 13. Критерий завершения

Incident можно считать практически закрытым, когда:

- пользователь подтвердил рабочий путь на нужных смартфонах/сетях;
- причина зафиксирована ограниченно, без чрезмерного вывода;
- секреты не опубликованы;
- СИСАДМИН получил redacted report или candidate через inbox/dispatch;
- unresolved registry/future work вынесены отдельно.

Если СИСАДМИН не дал acceptance, состояние маршрута остаётся `dispatched`, даже если практический результат уже работает. Да, в мире всё ещё существует разница между “оно работает” и “ответственный это принял”. Неприятно, но полезно.

## 14. Source boundary

Основано на:

- completed SHD VPN/V2rayNG/Hiddify incident report in `puev5691/wellbeing-hq`;
- ARH placement review with bounded follow-ups;
- SHD candidate experience cards in `puev5691/wellbeing-experience`;
- OPERATOR confirmation that Hiddify works and V2rayNG result stayed failed.

Не основано на fresh live server check. Для любого текущего server state требуется отдельная проверка.

---
КТО: SHD / ШАРДОВИК  
КОГДА: project_time omitted; trusted project-time source not used  
ДЛЯ ЧЕГО: подготовить candidate runbook для диагностики Android VPN client-layer incidents после V2rayNG/Hiddify resolution  
СТАТУС: runbook_candidate_for_SIS_review
