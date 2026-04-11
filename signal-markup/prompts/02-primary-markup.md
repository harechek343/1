# Промпт 02: Первичная разметка сигналов

## Назначение

В один проход построить:

- `units_registry`
- `signals_registry`
- `manual_queue`
- `trash`
- `stats`

## Вход

`normalized_units[]` + `B_total`

## Задача

Для каждой единицы:

1. сохранить полный отзыв в `units_registry`;
2. извлечь все атомарные сигналы;
3. назначить каждому сигналу одну категорию;
4. отправить спорное в `manual_queue`;
5. полностью пустое отправить в `trash`.

## Критические правила

1. Идентифицировать по `unit_index`, не по имени.
2. Один фрагмент = один сигнал.
3. Один сигнал = одна категория.
4. При сомнении "мусор или сигнал" — считать сигналом.
5. При сомнении по категории — в ручной разбор.
6. Полную цитату хранить один раз в `units_registry`.

## Формат outputs

### units_registry

```text
product_key: [ключ товара]
unit_index: #1
name: Анна
date: 2025-03-12
stars: 5
source: Wildberries
full_quote: «Крем потрясающий, запах держится весь день»
```

### signals_registry

```text
signal_id: [product_key]:#1:s1
product_key: [ключ товара]
unit_index: #1
name: Анна
category: позитив
signal_code: стойк.зап.
signal_fragment: «запах держится весь день»
secondary_flags: []
```

### manual_queue

```text
#12 | Елена | ручной разбор | «Ожидала плотную текстуру, а он жидкий как вода» | причина: expectation and negative fused
```

### trash

```text
#4 | Юля | мусор | «Супер!!!»
```

### stats

```text
B_total: ...
N_trash: ...
B_clean: ...
N_manual: ...
N_units_with_signals: ...
N_signals_total: ...
```

## Чего не делать

- не строить category reports;
- не группировать темы;
- не собирать TAGS;
- не дублировать full quote в каждом сигнале.
