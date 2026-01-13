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
updated_at: 2026-01-13T12:01:39Z
url: https://github.com/astral-sh/ruff/pull/22480
synced_at: 2026-01-13T12:25:13Z
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
| `invalid-return-type` | 1 | 1 | 5 |
| `invalid-argument-type` | 2 | 2 | 1 |
| `possibly-missing-attribute` | 3 | 0 | 1 |
| `unused-ignore-comment` | 3 | 1 | 0 |
| `invalid-await` | 2 | 0 | 0 |
| `unresolved-attribute` | 0 | 0 | 2 |
| **Total** | **11** | **4** | **9** |


**[Full report with detailed diff](https://dfb1a302.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://dfb1a302.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @astral-sh-bot[bot] on 2026-01-09 16:34_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5170 diagnostics
+ Found 5169 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2054 diagnostics
+ Found 2055 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14495 diagnostics
+ Found 14496 diagnostics


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

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:1 on 2026-01-13 11:28_

Could you add a test that we still emit these errors?

```py
from typing import Any

class DisjointBase1:
    __slots__ = ("a",)

class DisjointBase2:
    __slots__ = ("b",)

def f(ns: dict[str, Any]):
    cls1 = type("cls1", (DisjointBase1,), ns)
    cls2 = type("cls2", (DisjointBase2,), ns)

    # error: [instance-layout-conflict]
    cls3 = type("cls3", (cls1, cls2), {})

    # error: [instance-layout-conflict]
    class Cls4(cls1, cls2): ...

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/decorators/total_ordering.md`:1 on 2026-01-13 11:30_

If the `type()`-constructed class has a dynamic namespace, we should just assume that it provides a root ordering method (we should not emit an error if it's passed to `total_ordering()`). Can you add a test for that?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4707 on 2026-01-13 11:32_

Outdated comment -- we don't call it "functional" anymore, we call it "dynamic" now

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:1 on 2026-01-13 11:50_

Your code looks like it handles "partially dynamic" namespaces pretty well, but I don't see any explicit tests for things like these:

```py
def f(extra_attrs: dict[str, Any], y: str):
    X = type("X", (), {"a": 42, **extra_attrs})

    # Static attributes in the namespace dictionary have precise types,
    # but the dictionary was not entirely static, so other attributes
    # are still available and resolve to `Unknown`:
    reveal_type(X().a)  # revealed: Literal[42]
    reveal_type(X().whatever)  # revealed: Unknown

    Y = type("Y", (), {"a": 56, y: 72})
    reveal_type(Y().a)  # revealed: Literal[42]
    reveal_type(().whatever)  # revealed: Unknown
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4957 on 2026-01-13 11:59_

you could also do this as:

```suggestion
        // If the namespace is dynamic (not a literal dict) and the name isn't in `self.members`,
        // return Unknown since we can't know what attributes might be defined.
        self.members(db)
            .iter()
            .find_map(|(member_name, ty)| (name == member_name).then_some(*ty))
            .or_else(|| self.has_dynamic_namespace(db).then(Type::unknown))
            .map(Member::definitely_declared)
            .unwrap_or_default()
```

though we could also consider storing `members` as an `FxHashMap` (or `FxOrderMap`/`BTreeMap` if Salsa doesn't like an `FxHashMap` as a field of an interned struct) -- then the lookup of `name` in `self.members` would be `O(1)`

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:5017 on 2026-01-13 12:00_

As I mentioned in a comment on one of your test files, I think this should invoke `own_class_member`, so that we don't emit false positives on types with dynamic namespaces

---

_@AlexWaygood approved on 2026-01-13 12:01_

LGTM

---

_Review request for @Gankra removed by @AlexWaygood on 2026-01-13 12:01_

---

_Review request for @MichaReiser removed by @AlexWaygood on 2026-01-13 12:01_

---
