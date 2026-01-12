```yaml
number: 22480
title: "[ty] Support own instance members for `type(...)` classes"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: charlie/dyn-members
created_at: 2026-01-09T15:56:47Z
updated_at: 2026-01-12T20:42:34Z
url: https://github.com/astral-sh/ruff/pull/22480
synced_at: 2026-01-12T21:25:53Z
```

# [ty] Support own instance members for `type(...)` classes

---

_@charliermarsh_

## Summary

Addresses https://github.com/astral-sh/ruff/pull/22291#discussion_r2674467950.


---

_Comment by @astral-sh-bot[bot] on 2026-01-09 15:58_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Label `ecosystem-analyzer` added by @charliermarsh on 2026-01-09 16:00_

---

_Label `ty` added by @charliermarsh on 2026-01-09 16:00_

---

_Renamed from "Support own instance members for `type(...)` classes" to "[ty] Support own instance members for `type(...)` classes" by @charliermarsh on 2026-01-09 16:00_

---

_Comment by @astral-sh-bot[bot] on 2026-01-09 16:06_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 1 | 1 | 4 |
| `invalid-argument-type` | 0 | 0 | 1 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **2** | **1** | **5** |


**[Full report with detailed diff](https://d64f3c01.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://d64f3c01.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @astral-sh-bot[bot] on 2026-01-09 16:34_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-09 17:03_

---

_Review requested from @carljm by @charliermarsh on 2026-01-09 17:03_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-09 17:03_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-09 17:03_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-09 17:03_

---

_@AlexWaygood reviewed on 2026-01-09 17:21_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:107 on 2026-01-09 17:21_

If the class has a dynamic namespace dictionary (either the argument is not a dictionary literal, or not all the keys in the dictionary literal are string literals), should we just store a marker on the `DynamicClassLiteral` that records this, and say that it's a dynamic type that has arbitrary attributes available?

```py
from typing import Any
from ty_extensions import reveal_mro

class Base: ...

def f(attributes: dict[str, Any]):
    X = type("X", (Base,), attributes)

    reveal_mro(X)  # revealed: (<class 'Base'>, <class 'object'>)
    X().foo  # maybe this should be allowed?
```

I'd also be interested in this test, where a `TypedDict` is passed as the namespace argument... we know that an instance of a `TypedDict` can only ever have string-literal keys, but we don't know that it _only_ has those keys ([by default `TypedDict`s are "open"](https://typing.python.org/en/latest/spec/typeddict.html)):

```py
from typing import TypedDict

class Foo(TypedDict):
    z: int

def g(attributes: Foo):
    Y = type("Y", (), attributes)
```

Maybe in this case, we should infer that instances of `Y` have a `z` attribute of type `int`, but they also might have arbitrary other attributes as well (which are all `Unknown`), reflecting the fact that we know the `z` key in `Foo` instances are always of type `int` but that `Foo` instances can also have arbitrary other keys too?

As always, we don't have to tackle _all_ these questions _right_ now, but it would be great to add these tests so we record our current behaviour. I'm just trying to think through some edge cases

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-09 21:00_

---

_Review requested from @Gankra by @charliermarsh on 2026-01-09 21:00_

---

_Converted to draft by @charliermarsh on 2026-01-09 22:04_

---

_Marked ready for review by @charliermarsh on 2026-01-10 01:04_

---

_@AlexWaygood reviewed on 2026-01-10 13:07_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6069 on 2026-01-10 13:07_

Rather than checking whether any key is not a string-literal AST node, what we would usually do in this situation is check if the inferred _type_ of any key is not a string-literal _type_

---

_@AlexWaygood reviewed on 2026-01-10 13:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:155 on 2026-01-10 13:09_

We don't support the PEP-728 `closed=True` keyword argument to `TypedDict` yet (https://typing.python.org/en/latest/spec/typeddict.html#openness) but we should add some x-failing tests here so we remember to update our behaviour when we do add support for them.

I.e., the namespace for `X` should not be marked as dynamic in this situation:

```py
from typing import TypedDict

class MyNamespace(TypedDict, closed=True):
    x: int
    y: str

def _(ns: MyNamespace):
    X = type("X", (), ns)
```

---
