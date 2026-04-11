---
name: signal-category-analysis
description: Use when the user already has one or more signal registries from signal-markup and wants analytics for one chosen category such as positive, negative, expectations, doubts, comparisons, or scenarios. Best for clustering signals, counting themes across one or many products, and restoring quote context from units registries.
---

# Skill: signal-category-analysis

## Назначение

Этот skill делает именно то, что не должен делать `signal-markup`.

Он берёт готовые registry packages и делает аналитику по одной выбранной категории:

- позитив
- негатив
- ожидание
- сомнение
- сравнение
- сценарий

Может работать:

- по одному товару;
- по нескольким товарам сразу.

## Когда использовать

- у тебя уже есть `units_registry + signals_registry`;
- нужно прогнать только одну категорию;
- нужно собрать рынок по негативу, позитиву или другой призме;
- нужно поднять цитаты обратно через `unit_index`.

## Вход

Нужно передать:

- `category`
- один или несколько `registry packages`

Каждый пакет должен содержать:

- `product_key`
- `units_registry`
- `signals_registry`
- `stats`

## Выход

Skill возвращает:

1. `filtered_signals`
2. `themes`
3. `counts`
4. `quote_context_block`
5. `category_report`
6. `TAGS`

## Pipeline

```text
Input: chosen category + 1..N registry packages
    ↓
[01-load-registries] → unified registries
    ↓
[02-filter-category] → filtered_signals
    ↓
[03-cluster-and-count] → themes + counts
    ↓
[04-quote-context] → restored full quotes by refs
    ↓
[05-final-report] → category_report + TAGS
```

## Жёсткие запреты

1. Не искать сигналы заново в сырых отзывах.
2. Не менять категории сигналов.
3. Не терять связь с полной цитатой.
4. Не анализировать сразу все категории, если пользователь просит одну.

## Ссылки

### References
- [Input contract](references/input-contract.md)
- [Category rules](references/category-rules.md)
- [Output contract](references/output-contract.md)

### Prompts
- [01 — Load registries](prompts/01-load-registries.md)
- [02 — Filter category](prompts/02-filter-category.md)
- [03 — Cluster and count](prompts/03-cluster-and-count.md)
- [04 — Quote context](prompts/04-quote-context.md)
- [05 — Final report](prompts/05-final-report.md)
