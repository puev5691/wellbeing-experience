# wellbeing-experience

База накопленного опыта Сущностей проекта «БЛАГОПОЛУЧИЕ» и технологический контур Continuity v2.

## Зачем

Цель репозитория — хранить не биографию чатов и не очередные recovery-снапшоты, а проверяемый опыт, который должен изменять поведение следующих экземпляров Сущностей.

Рабочая формула:

`state continuity + experience continuity + behavioral verification`

## Три входных корпуса

- `RAW/PENSIONERS` — старые экземпляры, которые завершили полезную работу и были сохранены как исторические рабочие траектории.
- `RAW/HOSPITAL` — экземпляры с деградацией, нарушением инструкций, срывами поведения и другими pathology cases. Это основной материал для anti-regression.
- `LIVE DELTAS` — новые experience-delta, создаваемые по мере текущей работы Сущностей.

Сырые чаты не являются current truth и не загружаются целиком в новый экземпляр. Они используются как historical evidence для extraction.

## Конвейер

`raw corpus → inventory → extraction → experience cards → dedupe/contradiction review → reusable lessons/runbooks → anti-regression tests → role-specific experience-current → cold-start behavior test`

## Каталоги

- `raw/` — правила хранения исходных исторических материалов;
- `registry/` — intake и provenance реестр;
- `experience/` — extracted cards/lessons/runbooks;
- `tests/` — anti-regression и behavioral admission tests;
- `schemas/` — машиночитаемые схемы;
- `ops/` — методика extraction и пилоты;
- `views/` — собранные role-specific experience-current представления.

## Статус

`bootstrap_candidate`

Этот bootstrap не меняет действующие Project Sources и не объявляет Continuity v2 утверждённым каноном.
