# Промпт 09: Финальная сборка registry package

## Назначение

Собрать компактный пакет для downstream-аналитики.

## Вход

- `units_registry`
- `signals_registry`
- `manual_queue`
- `trash`
- `stats`

## Задача

Проверить целостность и вернуть финальный пакет.

## Проверки

### Обязательная

```text
B_total = N_trash + B_clean
```

### Дополнительные

- каждый сигнал ссылается на существующий `unit_index`;
- в `signals_registry` нет явных дублей одного и того же фрагмента;
- `manual_queue` не теряется;
- `trash` не смешивается с `B_clean`.

## Формат выхода

```text
FINAL REGISTRY PACKAGE

product_key: ...

STATS:
B_total: ...
N_trash: ...
B_clean: ...
N_manual: ...
N_units_with_signals: ...
N_signals_total: ...

UNITS_REGISTRY:
[список единиц]

SIGNALS_REGISTRY:
[список сигналов]

MANUAL_QUEUE:
[список]

TRASH:
[список]
```

## Чего не делать

- не строить category summaries;
- не делать clustering;
- не делать TAGS;
- не превращать пакет в длинный аналитический отчёт.
