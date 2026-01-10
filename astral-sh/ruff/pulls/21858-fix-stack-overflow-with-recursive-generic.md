```yaml
number: 21858
title: Fix stack overflow with recursive generic protocols (depth limit)
type: pull_request
state: merged
author: carljm
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: cjm/protoso2
created_at: 2025-12-09T02:45:00Z
updated_at: 2025-12-09T17:05:20Z
url: https://github.com/astral-sh/ruff/pull/21858
synced_at: 2026-01-10T16:42:11Z
```

# Fix stack overflow with recursive generic protocols (depth limit)

---

_Pull request opened by @carljm on 2025-12-09 02:45_

## Summary

This fixes https://github.com/astral-sh/ty/issues/1736 where recursive generic protocols with growing specializations caused a stack overflow.

The issue occurred with protocols like:
```python
class C[T](Protocol):
    a: 'C[set[T]]'
```

When checking `C[set[int]]` against e.g. `C[Unknown]`, member `a` requires checking `C[set[set[int]]]`, which requires `C[set[set[set[int]]]]`, etc. Each level has different type specializations, so the existing cycle detection (using full types as cache keys) didn't catch the infinite recursion.

This fix adds a simple recursion depth limit (64) to the CycleDetector. When the depth exceeds the limit, we return the fallback value (assume compatible) to safely terminate the recursion.

This is a bit of a blunt hammer, but it should be broadly effective to prevent stack overflow in any nested-relation case, and it's hard to imagine that non-recursive nested relation comparisons of depth > 64 exist much in the wild.

## Test Plan

Added mdtest.


---

_Label `ty` added by @carljm on 2025-12-09 02:45_

---

_Comment by @astral-sh-bot[bot] on 2025-12-09 02:47_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-09 02:48_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5545 diagnostics
+ Found 5544 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-09 02:49_

---

_Comment by @astral-sh-bot[bot] on 2025-12-09 02:58_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 2 | 0 | 0 |
| **Total** | **2** | **0** | **0** |

**[Full report with detailed diff](https://cjm-protoso2.ecosystem-663.pages.dev/diff)** ([timing results](https://cjm-protoso2.ecosystem-663.pages.dev/timing))




---

_Marked ready for review by @carljm on 2025-12-09 03:06_

---

_Review requested from @AlexWaygood by @carljm on 2025-12-09 03:06_

---

_Review requested from @sharkdp by @carljm on 2025-12-09 03:06_

---

_Review requested from @dcreager by @carljm on 2025-12-09 03:06_

---

_Comment by @mtshiba on 2025-12-09 07:08_

Can't we implement a salsa query with cycle handling instead of using `CycleDetector`?

---

_Comment by @carljm on 2025-12-09 08:31_

What would the inputs to the Salsa query be? The issue here is that we have a different pair of types every time, because the specialization changes every time. So I don't think Salsa will detect it as a cycle either, if we just make `satisfies_protocol` a query. But I can try it tomorrow and see. 

---

_Comment by @MichaReiser on 2025-12-09 10:11_

> What would the inputs to the Salsa query be? The issue here is that we have a different pair of types every time, because the specialization changes every time. So I don't think Salsa will detect it as a cycle either, if we just make satisfies_protocol a query. But I can try it tomorrow and see.

I think a depth limit is fine for as long as it is deterministic. While salsa's cycle handling is convenient, it does come at a cost of higher memory usage (because it's persistent) and cycle handling is more complex than a hard limit in the visitor

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/cyclic.rs`:79 on 2025-12-09 10:11_

```suggestion
    depth: Cell<u32>,
```

---

_@MichaReiser approved on 2025-12-09 10:12_

---

_Comment by @carljm on 2025-12-09 17:05_

> Can't we implement a salsa query with cycle handling

I tried to test this out, but there are some problems with making anything in the `has_relation_to` method chain a Salsa query, since we pass around a hashset of inferable typevars, which is not hashable... Not going to pursue this more at the moment, since I don't think it would fix the stack overflow anyway.

---

_Merged by @carljm on 2025-12-09 17:05_

---

_Closed by @carljm on 2025-12-09 17:05_

---

_Branch deleted on 2025-12-09 17:05_

---
