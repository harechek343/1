# Контракт выходных данных

Этот skill должен возвращать не аналитический отчёт, а `registry package`.

## 1. units_registry

Одна запись = одна исходная единица.

Формат:

```text
product_key: offscrub-hand-cream
unit_index: #12
name: Елена
date: 2025-03-10
stars: 1
source: Wildberries
full_quote: «Ожидала плотную текстуру, а он жидкий как вода»
```

## 2. signals_registry

Одна запись = один атомарный сигнал.

Формат:

```text
signal_id: offscrub-hand-cream:#12:s1
product_key: offscrub-hand-cream
unit_index: #12
name: Елена
category: негатив
signal_code: жидк.текст.
signal_fragment: «он жидкий как вода»
secondary_flags: [has_expectation]
```

Важно:

`signals_registry` не обязан каждый раз повторять `full_quote`, потому что полная цитата уже хранится в `units_registry`.

## 3. manual_queue

Одна запись = один спорный фрагмент.

Формат:

```text
#12 | Елена | ручной разбор | «Ожидала плотную текстуру, а он жидкий как вода» | причина: expectation and negative fused
```

## 4. trash

Одна запись = одна пустая/неинформативная единица.

Формат:

```text
#44 | Марина | мусор | «Супер!!!»
```

## 5. stats

```text
B_total: 243
N_trash: 13
B_clean: 230
N_manual: 6
N_units_with_signals: 224
N_signals_total: 581
```

## 6. final registry package

Итоговый пакет должен содержать:

- `product_key`
- `units_registry`
- `signals_registry`
- `manual_queue`
- `trash`
- `stats`

## Принцип downstream-совместимости

Следующий skill должен уметь:

1. брать `signals_registry`;
2. фильтровать нужную категорию;
3. по `product_key + unit_index` поднимать полную цитату из `units_registry`.

Именно так восстанавливается контекст без постоянного дублирования цитат.
