```yaml
number: 14927
title: "[red-knot] Understand `typing.Tuple`"
type: pull_request
state: merged
author: InSyncWithFoo
labels: []
assignees: []
merged: true
base: main
head: rk-typing-tuple
created_at: 2024-12-12T00:05:05Z
updated_at: 2024-12-12T01:26:30Z
url: https://github.com/astral-sh/ruff/pull/14927
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] Understand `typing.Tuple`

---

_@InSyncWithFoo_

## Summary

Resolves #14916.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2024-12-12 00:05_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2024-12-12 00:05_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2024-12-12 00:05_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2024-12-12 00:05_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2214 on 2024-12-12 00:10_

Oh I think I missed this in your earlier PR adding support for `typing.Type`. Shouldn't both of these branches be `KnownClass::SpecialForm`? The symbols are defined as instances of `_SpecialForms` in typeshed: https://github.com/python/typeshed/blob/2666d3b5b06b5436470017d46d01929da70ab5f8/stdlib/typing.pyi#L207-L212

```suggestion
            Self::Tuple => KnownClass::SpecialForm,
            Self::Type => KnownClass::SpecialForm,
```

---

_Comment by @github-actions[bot] on 2024-12-12 00:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-12-12 00:11_

Thanks!

---

_@AlexWaygood reviewed on 2024-12-12 00:12_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2214 on 2024-12-12 00:12_

In fact, I think this might be what's now causing the new false positive error in the `tomllib` benchmark (red-knot no longer thinks the symbol `typing.Tuple` is an instance of `_SpecialForm`, so it no longer thinks the `typing.Tuple` variable has a `__getitem__` method)

---

_@InSyncWithFoo reviewed on 2024-12-12 00:38_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/src/types.rs`:2214 on 2024-12-12 00:38_

I consulted the runtime, which said these don't have `_SpecialForm` in their MROs:

```py
Python 3.14.0a2 (tags/v3.14.0a2:add43c3, Nov 19 2024, 16:57:13) [MSC v.1942 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> from typing import Tuple, Type
>>> [cls.__name__ for cls in type(Tuple).__mro__]
['_TupleType', '_SpecialGenericAlias', '_NotIterable', '_BaseGenericAlias', '_Final', 'object']
>>> [cls.__name__ for cls in type(Type).__mro__]
['_SpecialGenericAlias', '_NotIterable', '_BaseGenericAlias', '_Final', 'object']
```

...unlike, say, `Annotated`:

```pycon
>>> from typing import Annotated
>>> [cls.__name__ for cls in type(Annotated).__mro__]
['_TypedCacheSpecialForm', '_SpecialForm', '_Final', '_NotIterable', 'object']
```

The specification unfortunately [doesn't have an exhaustive list](https://typing.readthedocs.io/en/latest/spec/glossary.html#term-special-form) for us to check against, but I guess `SpecialForm` is indeed the way to go, considering that both `Any` and `TypedDict` are listed there.

```pycon
>>> [cls.__name__ for cls in type(Any).__mro__]
['_AnyMeta', 'type', 'object']
>>> [cls.__name__ for cls in type(TypedDict).__mro__]
['function', 'object']
```

---

_@AlexWaygood reviewed on 2024-12-12 00:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2214 on 2024-12-12 00:57_

Yeah, for better or worse, typeshed doesn't reflect all the gory details of the runtime implementation right now. In some cases, like this, it simplifies a little. It might seem strange, but it's more important here that we follow what typeshed says the facts are rather than follow the actually true facts as they are at runtime.

---

_@AlexWaygood approved on 2024-12-12 00:57_

Nice, thanks!

---

_Merged by @AlexWaygood on 2024-12-12 00:58_

---

_Closed by @AlexWaygood on 2024-12-12 00:58_

---

_Branch deleted on 2024-12-12 01:26_

---
