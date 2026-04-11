---
name: signal-markup
description: "Use when the user needs fast extraction of atomic buyer signals from a Markdown table of reviews or questions for one product. Single-pass extraction into a master registry with emoji-coded full quotes and a flat signal list. No category analytics, no weighting, no product-card decisions."
---

# Skill: signal-markup

## Назначение

Быстрое извлечение атомарных сигналов из отзывов. Один проход. Один промпт. Два блока на выходе.

## Когда использовать

- есть массив отзывов/вопросов по одному товару;
- нужна полная атомарная разметка;
- нужно сохранить полный контекст цитат;
- нужен быстрый реестр для дальнейшей аналитики.

## НЕ использовать для

- тематической аналитики (темы, TAGS, группировки);
- рыночного сравнения нескольких товаров;
- buyer interpretation;
- противовесного фильтра;
- решений по карточке.

## Вход

Markdown-таблица с полями: `#` (unit_index), `Имя`, `Текст`.

Желательно: `Дата`, `Звёзды`.

Опционально: `Товар`, `Источник`.

Подробнее: [references/input-format.md](references/input-format.md)

## Выход

Один документ `master_registry.md` с 5 блоками:

- Блок A: Размеченные цитаты (emoji + жирное выделение)
- Блок B: Плоский реестр сигналов
- Блок C: Ручной разбор
- Блок D: Мусор
- Блок E: Статистика

Подробнее: [references/output-format.md](references/output-format.md)

## Pipeline flow

```
Вход (Markdown-таблица, 1 товар)
    ↓
[extract-signals] — единственный промпт, один проход
    ↓
master_registry.md (5 блоков)
    ↓
[Опционально] Человек решает ручной разбор → patch
```

## Emoji-легенда

| Emoji | Категория |
|-------|-----------|
| 🟢 | позитив |
| 🔴 | негатив |
| ⚪ | ожидание |
| 🔵 | сомнение |
| 🟣 | сравнение |
| 🟠 | сценарий |

## Жёсткие запреты

1. Не придумывать сигналы.
2. Не пересказывать своими словами.
3. Не дублировать один фрагмент в двух категориях.
4. Не останавливаться из-за ручного разбора.
5. Не строить category analytics в этом skill.
6. Не группировать в темы.
7. Не считать TAGS.
8. Не делать красивый итоговый отчёт.

## Инварианты

- Один отзыв может содержать сколько угодно сигналов.
- Один и тот же кусок текста нельзя использовать дважды.
- Каждый сигнал попадает только в одну категорию.
- Если категория неочевидна — в ручной разбор.
- При сомнении «мусор или сигнал» — считать сигналом.
- Каждый отзыв в Блоке A ровно один раз (не дублируется).

## Ссылки

### References
- [Формат входа](references/input-format.md)
- [Формат выхода](references/output-format.md)
- [Категории](references/categories.md)
- [Протокол составных сигналов](references/composite-signal-protocol.md)
- [Спорные случаи](references/edge-cases.md)

### Prompts
- [extract-signals](prompts/extract-signals.md)
