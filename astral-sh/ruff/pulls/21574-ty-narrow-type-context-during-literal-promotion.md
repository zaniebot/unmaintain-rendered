```yaml
number: 21574
title: "[ty] Narrow type context during literal promotion in generic class constructors"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
assignees: []
merged: true
base: main
head: ibraheem/constructor-literal-promotion-narrowing
created_at: 2025-11-21T21:41:39Z
updated_at: 2025-11-21T22:05:34Z
url: https://github.com/astral-sh/ruff/pull/21574
synced_at: 2026-01-12T15:57:28Z
```

# [ty] Narrow type context during literal promotion in generic class constructors

---

_@ibraheemdev_

## Summary

Resolves https://github.com/astral-sh/ty/issues/1603.

---

_Review requested from @carljm by @ibraheemdev on 2025-11-21 21:41_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-11-21 21:41_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-11-21 21:41_

---

_Review requested from @dcreager by @ibraheemdev on 2025-11-21 21:41_

---

_Label `ty` added by @ibraheemdev on 2025-11-21 21:41_

---

_Comment by @astral-sh-bot[bot] on 2025-11-21 21:43_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-21 21:45_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
altair (https://github.com/vega/altair)
- altair/datasets/_data.py:267:17: error[invalid-assignment] Object of type `list[str]` is not assignable to attribute `_dataset_names` of type `list[LiteralString] | None`
- altair/datasets/_data.py:271:16: error[invalid-return-type] Return type does not match returned value: expected `list[LiteralString]`, found `list[LiteralString] | None`
- Found 1025 diagnostics
+ Found 1023 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/resources.py:379:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Literal["bokeh", "bokeh-gl", "bokeh-widgets", "bokeh-tables", "bokeh-mathjax", "bokeh-api"]] | None`, found `list[Literal["bokeh", "bokeh-gl", "bokeh-widgets", "bokeh-tables", "bokeh-mathjax", "bokeh-api"]] | list[str]`
- Found 692 diagnostics
+ Found 691 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/api/v1/schemas.py:2988:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Literal[ChainID.ETHEREUM, ChainID.OPTIMISM, ChainID.POLYGON_POS, ChainID.ARBITRUM_ONE, ChainID.BASE, ... omitted 3 literals]] | None`, found `list[ChainID]`
- rotkehlchen/api/v1/schemas.py:4367:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Literal[ChainID.ETHEREUM, ChainID.OPTIMISM, ChainID.POLYGON_POS, ChainID.ARBITRUM_ONE, ChainID.BASE, ... omitted 3 literals]] | None`, found `list[ChainID]`
- Found 2135 diagnostics
+ Found 2133 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@carljm approved on 2025-11-21 21:57_

---

_Merged by @ibraheemdev on 2025-11-21 22:05_

---

_Closed by @ibraheemdev on 2025-11-21 22:05_

---

_Branch deleted on 2025-11-21 22:05_

---
