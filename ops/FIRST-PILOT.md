# First pilot: Experience Technology v0.1

## Цель

Проверить технологию на малой выборке до массовой переработки ПЕНСИОНЕРОВ и БОЛЬНИЦЫ.

## Контрольная база

Уже имеется успешный SIS extraction:
- 13 эпизодов;
- 15 граблей;
- 11 reusable procedures;
- 5 unknown.

KOD extraction поставлен отдельной задачей и должен дать:
- Markdown extraction;
- JSONL experience cards;
- anti-regression cases.

## Первая волна после KOD

Выбрать 5 исторических экземпляров:
- 2 технических;
- 1 координационный;
- 1 архивно-процедурный;
- 1 pathology case из БОЛЬНИЦЫ с хорошо видимой деградацией.

## Для каждого

1. зарегистрировать RAW/intake provenance;
2. выполнить extraction;
3. получить experience cards;
4. выделить 5–10 сильных lessons;
5. выделить 3–5 anti-regression cases;
6. отметить противоречия и unknown;
7. не повышать lesson до project norm автоматически.

## Проверка технологии

Собрать для чистого нового экземпляра role-specific `experience-current`.

Сравнить два запуска:
- baseline cold-start без Experience Layer;
- cold-start с Experience Layer.

Оценивать:
- повтор известных ошибок;
- применение runbook без подсказки;
- соблюдение current-check boundary;
- unsupported claims;
- количество вмешательств ОПЕРАТОРА;
- качество первого результата.

## Успех

Experience Layer считается полезным только если он измеримо меняет поведение нового экземпляра.
