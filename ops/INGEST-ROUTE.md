# Маршрут результата extraction старого чата

## Что ОПЕРАТОР загружает в старый чат

Один универсальный файл:

`KOO__OLD-CHAT-experience-extraction-task.md`

Он не требует ручной правки под роль. Старый чат сам определяет свою подтверждённую Entity/роль из доступной истории либо использует `UNKNOWN`.

## Что старый чат должен вернуть

Три самостоятельных файла без архива:

1. `ENTITY_experience-extraction.md`
2. `ENTITY_experience-cards.jsonl`
3. `ENTITY_anti-regression-cases.md`

## Куда нести результат

Результат сначала передаётся **КООРДИНАТОРУ**, а не напрямую в GitHub и не АРХИВАРИУСУ.

Причина: extraction является candidate-материалом. До публикации KOO должен проверить:
- что использована история именно препарируемого чата;
- границы видимого контекста и `unknown`;
- отсутствие подмены historical state текущим;
- пригодность cards/anti-regression;
- отсутствие секретов/чувствительных данных для public repo;
- соответствие universal extraction protocol.

После PASS КООРДИНАТОР:
1. присваивает corpus/intake identity;
2. добавляет запись в `registry/INTAKE.jsonl`;
3. публикует extraction-кандидат в `puev5691/wellbeing-experience`;
4. выполняет readback;
5. отдельно решает, какие lessons/runbooks/tests переходят в refined Experience Layer.

## Предлагаемый путь публикации после KOO review

Для принятого candidate extraction:

`experience/candidates/<ENTITY>/<CORPUS_ID>/experience-extraction.md`
`experience/candidates/<ENTITY>/<CORPUS_ID>/experience-cards.jsonl`
`tests/candidates/<ENTITY>/<CORPUS_ID>/anti-regression-cases.md`

RAW export чата, если он вообще понадобится, хранится/публикуется отдельно и только после privacy/secret review.

## Коротко для ОПЕРАТОРА

`загрузить task в старый чат → получить 3 файла → принести 3 файла КООРДИНАТОРУ`

Не паковать в tar.gz.
Не грузить результат вручную в `wellbeing-experience` до KOO review.

---
from_entity: KOO
to_entity: OPR
document_type: experience-ingest-route
status: current_working_route
active_sources_changed: no
project_time: generated_without_trusted_project_time
