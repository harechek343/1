# Registry Package Template

## Метаданные

- product_key: `...`
- товар: `...`
- источник: `...`
- дата прогона: `...`

## Stats

- `B_total`: `...`
- `N_trash`: `...`
- `B_clean`: `...`
- `N_manual`: `...`
- `N_units_with_signals`: `...`
- `N_signals_total`: `...`

## Units Registry

```text
product_key: ...
unit_index: #...
name: ...
date: ...
stars: ...
source: ...
full_quote: «...»
```

## Signals Registry

```text
signal_id: ...
product_key: ...
unit_index: #...
name: ...
category: ...
signal_code: ...
signal_fragment: «...»
secondary_flags: [...]
```

## Manual Queue

```text
#... | ... | ручной разбор | «...» | причина: ...
```

## Trash

```text
#... | ... | мусор | «...»
```
