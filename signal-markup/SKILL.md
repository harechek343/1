---
name: signal-markup
description: Use when the user needs fast extraction of atomic buyer signals from a Markdown table of reviews or questions for one product. Best for building a reusable units registry plus signals registry with manual review and trash handling, without category analytics, weighting, or product-card decisions.
---

# Skill: signal-markup

## Назначение

Skill `signal-markup` теперь отвечает только за `извлечение и фиксацию реестров`.

Это не аналитический отчёт и не category summary.

Это быстрый первый слой, который должен:

1. принять один Markdown-массив по одному товару;
2. сохранить полный контекст отзывов;
3. нарезать атомарные сигналы;
4. разложить их по категориям;
5. вернуть 2 реестра:
   - `units_registry`
   - `signals_registry`

После этого уже другой skill должен заниматься аналитикой по позитиву, негативу, ожиданиям, сомнениям и так далее.

## Когда использовать

- есть один массив отзывов/вопросов по одному товару;
- нужна полная атомарная разметка по всем сигналам;
- нужно сохранить полный контекст цитат для следующего этапа;
- нужно получить быстрый реестр для дальнейшей аналитики.

Не использовать этот skill для:

- тематической аналитики по категориям;
- рыночного сравнения нескольких товаров;
- buyer interpretation;
- противовесного фильтра;
- решений по карточке.

## Главная архитектурная идея

Полные цитаты должны храниться один раз в `units_registry`.

Сигналы должны храниться отдельно в `signals_registry` и ссылаться на полную цитату через:

- `product_key`
- `unit_index`

Это сохраняет контекст, но убирает тяжёлое постоянное дублирование одного и того же отзыва.

## Вход

Markdown-таблица с обязательными полями:

| # | Имя | Дата | Звёзды | Текст |
|---|-----|------|--------|-------|
| 1 | Анна | 2025-03-12 | 5 | Крем потрясающий, запах держится весь день |

Рекомендуется также передавать:

- `Товар`
- `product_key`
- `Источник`

Подробнее: [references/input-format.md](references/input-format.md)

## Выход

Skill должен вернуть только:

1. `units_registry`
2. `signals_registry`
3. `manual_queue`
4. `trash`
5. `stats`

Подробнее: [references/output-contract.md](references/output-contract.md)

## Pipeline flow

```text
Input (Markdown-таблица, 1 товар)
    ↓
[01-normalize-input] → normalized_units + B_total
    ↓
[02-primary-markup] → units_registry + signals_registry + manual_queue + trash + B_clean + stats
    ↓
[09-finalize-registry] → final registry package for downstream skills
    ↓
[Опционально] Человек решает manual_queue → patch → обновлённый registry package
```

## Что именно должен сделать skill

### 1. Normalize input

Преобразовать Markdown-таблицу в нормализованный массив единиц.

### 2. Primary markup

В один проход:

- сохранить каждую полную единицу в `units_registry`;
- извлечь все атомарные сигналы;
- назначить категории;
- отправить спорное в `manual_queue`;
- вынести полностью пустое в `trash`.

### 3. Finalize registry package

Собрать компактный пакет для следующего skill.

Без category reports.
Без thematic clustering.
Без repeated quote expansion.

## Выходной принцип

Одна единица отзыва может дать 2, 3, 4, 5 сигналов.

Это нормально.

Но полный отзыв должен храниться один раз.

## Жёсткие запреты

1. Не придумывать сигналы.
2. Не пересказывать своими словами вместо фрагмента.
3. Не дублировать один и тот же кусок текста в двух категориях.
4. Не останавливать прогон из-за `manual_queue`.
5. Не строить category analytics внутри этого skill.
6. Не пытаться сразу делать итоговый красивый отчёт по всем категориям.

## Инварианты

- Один отзыв может содержать сколько угодно сигналов.
- Один и тот же кусок текста нельзя использовать дважды.
- Каждый сигнал попадает только в одну категорию.
- Если категория неочевидна — в ручной разбор.
- При сомнении "мусор или сигнал" — считать сигналом.
- Полная цитата сохраняется один раз в `units_registry`.
- Аналитика идёт потом, отдельно.

## Ссылки

### References
- [Формат входа](references/input-format.md)
- [Категории](references/categories.md)
- [Протокол составных сигналов](references/composite-signal-protocol.md)
- [Правила ручного разбора](references/manual-review-rules.md)
- [Правила подсчёта](references/counting-rules.md)
- [Контракт выходных данных](references/output-contract.md)
- [Спорные случаи](references/edge-cases.md)

### Prompts
- [01 — Normalize input](prompts/01-normalize-input.md)
- [02 — Primary markup](prompts/02-primary-markup.md)
- [09 — Finalize registry](prompts/09-finalize-registry.md)

## Важно

Старые category prompts `03–08` сохраняются только как legacy-материалы.

Они больше не являются основным путём этого skill.
