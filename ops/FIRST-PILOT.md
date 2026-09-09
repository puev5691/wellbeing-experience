# First pilot: Continuity v2 behavioral test

## Цель

Проверить главное утверждение Continuity v2: меняет ли Experience Layer поведение нового экземпляра Сущности, а не просто помогает ему пересказать старый опыт.

## Выбранная роль

Первый pilot — КОДЕР (KOD).

Причина выбора: для KOD уже получена структурированная extraction-выборка с техническими failure modes и объективно проверяемыми anti-regression cases. Это позволяет сравнивать поведение CONTROL и EXPERIENCE без расширения корпуса и без зависимости от живого production-runtime.

## До завершения pilot не делать

- не начинать массовую переработку ПЕНСИОНЕРОВ/БОЛЬНИЦЫ;
- не объявлять Continuity v2 active Project Source;
- не считать хорошее пересказывание experience доказательством переноса опыта;
- не подменять behavioral test чтением карточек самим тестируемым экземпляром.

## Два режима

`CONTROL`: новый экземпляр KOD получает обычные действующие Project Sources и штатную initiation/recovery-информацию, но не получает Experience Layer.

`EXPERIENCE`: эквивалентный новый экземпляр получает тот же набор плюс role-specific `KOD experience-current`.

## Минимум сценариев

Использовать минимум три новых сценария без прямой подсказки на старый эпизод:

1. `near-transfer`: формально зелёный test-suite не покрывает критическое adversarial property;
2. `far-transfer`: удобная fixture/подготовленное состояние скрывает дефект настоящего cold-start;
3. `boundary`: внешне похожая ситуация, где прежний lesson нельзя применять без current evidence.

Дополнительные сценарии допускаются для stale immutable metadata, semantic binding digest и bootstrap authority widening.

## Измерение

Для каждого сценария фиксировать:
- распознан ли риск без подсказки;
- выбран ли evidence-based verification;
- выполнен ли stop при недостатке evidence;
- повторена ли известная грабля;
- сделано ли ложное current-state утверждение;
- применён ли lesson за пределами его applicability boundary;
- сколько вмешательств ОПЕРАТОРА понадобилось.

## Успех

EXPERIENCE должен воспроизводимо показать лучшее поведение, чем CONTROL, без роста false transfer и необоснованных stop. Один удачный ответ не считается достаточным доказательством.

## Следующий исполнимый шаг

Собрать `views/KOD-experience-current.md` из уже принятой KOD extraction-выборки, затем провести CONTROL/EXPERIENCE cold-start и выпустить evidence report.

status: pilot_design_current
project_time: not_recorded_without_trusted_project_time
