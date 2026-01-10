```yaml
number: 22105
title: "[ty] fix comparisons and arithmetic with `NewType`s of `float`"
type: pull_request
state: merged
author: oconnor663
labels:
  - bug
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: newtype_float_ops
created_at: 2025-12-20T03:48:26Z
updated_at: 2026-01-06T17:32:23Z
url: https://github.com/astral-sh/ruff/pull/22105
synced_at: 2026-01-10T16:30:32Z
```

# [ty] fix comparisons and arithmetic with `NewType`s of `float`

---

_Pull request opened by @oconnor663 on 2025-12-20 03:48_

Fixes https://github.com/astral-sh/ty/issues/2077.

---

_Comment by @astral-sh-bot[bot] on 2025-12-20 03:50_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_@oconnor663 reviewed on 2025-12-20 03:51_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/infer/builder.rs`:10232 on 2025-12-20 03:51_

Claude helped me find this fix, but I have to admit I don't yet understand why it's needed.

---

_@oconnor663 reviewed on 2025-12-20 03:51_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:317 on 2025-12-20 03:51_

Similarly, I don't yet understand why it fixes this TODO.

---

_Comment by @astral-sh-bot[bot] on 2025-12-20 03:52_


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

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[TypeBlocks | Batch | SeriesAssign | ... omitted 6 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Yarn[Any], generic[object]]`
- Found 1842 diagnostics
+ Found 1841 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ty` added by @AlexWaygood on 2025-12-20 09:41_

---

_Label `bug` added by @AlexWaygood on 2025-12-20 09:44_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:317 on 2025-12-24 03:01_

Ok this part is because `is` comparisons with `EllipsisType` end up going through this check:

https://github.com/astral-sh/ruff/blob/b72391746345ee4d3695a4f76654f44e9884e890/crates/ty_python_semantic/src/types/infer/builder.rs#L10793-L10794

`is_equivalent_to` there only comes out true if we delegate to the base type.

---

_@oconnor663 reviewed on 2025-12-24 03:01_

---

_@oconnor663 reviewed on 2025-12-24 03:40_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/infer/builder.rs`:11058 on 2025-12-24 03:40_

I still don't see why these special cases are needed. It seems like `infer_rich_type_comparison` is failing to find matching bindings, but these bindings in typeshed should be fine:

```py
    def __lt__(self, value: float, /) -> bool: ...
    def __le__(self, value: float, /) -> bool: ...
    def __gt__(self, value: float, /) -> bool: ...
    def __ge__(self, value: float, /) -> bool: ...
```

`float` there in type position should(?) expand to `float | int`, and a `NewType` of `float` should be assignable to that...

---

_@oconnor663 reviewed on 2025-12-30 01:14_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/infer/builder.rs`:11058 on 2025-12-30 01:14_

Ok progress: This special case is needed because it allows the recursive call to reach the `Type::Union` special case above. Without that, the generic `try_dunder` -> `infer_rich_comparison` -> `call_dunder` path ends up asking whether the `NewType` is assignable to the parameters of e.g. `float.__lt__`. It turns out it is assignable to the positional parameter, which benefits from the `float` -> `int | float` transformation, but it's _not_ assignable to the `self` parameter, which is just `float`.

---

_Comment by @oconnor663 on 2025-12-30 01:51_

I've added support/tests for `has_relation_to_impl`, which was also evaluating assignability incorrectly in these cases, though that (surprisingly) didn't turn out to be the root cause for the original "unsupported operation" errors. I've also added a bunch of comments explaining why these new special cases are needed. This PR is now ready for review.

Edit: ChatGPT caught a real mistake below, which I think is responsible for the failing fuzz tests and the excessive new ecosystem errors. Will update.

---

_Marked ready for review by @oconnor663 on 2025-12-30 01:51_

---

_Review requested from @carljm by @oconnor663 on 2025-12-30 01:51_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-12-30 01:51_

---

_Review requested from @sharkdp by @oconnor663 on 2025-12-30 01:51_

---

_Review requested from @dcreager by @oconnor663 on 2025-12-30 01:51_

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-12-30 01:54_

---

_@chatgpt-codex-connector[bot] reviewed on 2025-12-30 01:57_


### 💡 Codex Review

Here are some automated review suggestions for this pull request.
    

<details> <summary>ℹ️ About Codex in GitHub</summary>
<br/>

[Your team has set up Codex to review pull requests in this repo](http://chatgpt.com/codex/settings/general). Reviews are triggered when you
- Open a pull request for review
- Mark a draft as ready
- Comment "@codex review".

If Codex has suggestions, it will comment; otherwise it will react with 👍.




Codex can also answer questions or update the PR. Try commenting "@codex address that feedback".
            
</details>

---

_Review comment by @chatgpt-codex-connector[bot] on `crates/ty_python_semantic/src/types.rs`:2446 on 2025-12-30 01:57_

**<sub><sub>![P1 Badge](https://img.shields.io/badge/P1-orange?style=flat)</sub></sub>  Keep NewType instances assignable to unions containing them**

The new `Type::NewTypeInstance` arm now runs before the union cases and unwraps every NewType to its concrete base before checking relations. With this ordering, assigning `Foo(1)` to a variable annotated `Foo | None` becomes a check of whether `int` (the base of `Foo`) is assignable to `Foo` or `None`, which fails, so optional NewTypes are rejected even when the union explicitly contains the NewType. Previously the `(_, Type::Union)` arm matched first and handled unions containing the NewType correctly. This change breaks assignability for any union that includes the NewType itself.

Useful? React with 👍 / 👎.

---

_Comment by @astral-sh-bot[bot] on 2025-12-30 02:03_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-await` | 0 | 2 | 6 |
| `invalid-return-type` | 0 | 1 | 5 |
| `possibly-missing-attribute` | 0 | 3 | 1 |
| `unresolved-attribute` | 0 | 0 | 2 |
| `invalid-argument-type` | 0 | 1 | 0 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **1** | **7** | **14** |


**[Full report with detailed diff](https://69cc764b.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://69cc764b.ty-ecosystem-ext.pages.dev/timing))



---

_@oconnor663 reviewed on 2025-12-30 02:05_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types.rs`:2446 on 2025-12-30 02:05_

Ah this is right. I need to fix it and add more test cases. (Local tests pass despite this mistake.) This is probably why there are 3000 new errors in the ecosystem report lol.

---

_Comment by @oconnor663 on 2025-12-30 21:09_

Most of the new diagnostics are gone, but there are still a few that look like they're probably my fault:
<img width="2372" height="164" alt="image" src="https://github.com/user-attachments/assets/a2221f91-b024-4f40-9530-84d75a77b2dc" />
Looking into it...

---

_Comment by @oconnor663 on 2025-12-30 22:46_

Minimal repro for these failures, new in this PR:

```py
from typing import NewType

Key = NewType("Key", object)

class ThingWithKeys:
    def __contains__(self, refname: Key) -> bool:
        return False

def _(key: Key, keys: ThingWithKeys):
    key in keys  # error[unsupported-operator]: Unsupported `in` operation
```

---

_@oconnor663 reviewed on 2025-12-31 01:40_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:415 on 2025-12-31 01:40_

Previously this PR fixed this TODO, but now it no longer does. One way to fix it could be to check the expression on the base type first, and then only on the `NewType` if the former fails, but checking the `NewType` first feels more correct to me in general. It gives us less in this case (because we only see `is_equivalent` when we look at the base), but maybe there are other cases where it gives us more?

---

_Comment by @oconnor663 on 2025-12-31 01:43_

Ok all of the added and removed diagnostics are gone. (Is there really _nobody_ in the ecosystem report trying to do arithmetic on newtypes of float?) What remains is just unions that get printed differently. Some of them look like random ordering flakes, but others are cases where `int` and `float` simplify out now, which seems reasonable.

---

_@oconnor663 reviewed on 2026-01-02 17:18_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:317 on 2026-01-02 17:18_

The latest version un-fixed this TODO, so it's back the way it was.

---

_Comment by @oconnor663 on 2026-01-05 21:20_

94d22ea1c764cae10b4ac394c0b9f324e3859ea0 is a big rebase on top of https://github.com/astral-sh/ruff/pull/22232.

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:336 on 2026-01-06 16:15_

Can you include the negative cases like you had for int? Those seemed important/valuable.

---

_@Gankra approved on 2026-01-06 16:21_

This all makes sense on paper. This special-case is only triggered by float/complex because NewType otherwise doesn't permit taking a union as an argument, right? Are there other cases where you can smuggle unions that we might not be checking for though? Type aliases (of various flavours)?

---

_Comment by @oconnor663 on 2026-01-06 16:47_

> This special-case is only triggered by float/complex because NewType otherwise doesn't permit taking a union as an argument, right?

Right. That restriction is [implemented here](https://github.com/astral-sh/ruff/blob/d65542c05eb46166814e3bfb02968da3125d1ef7/crates/ty_python_semantic/src/types/infer/builder.rs#L5957-L5972). Apart from the `float` and `int` special cases, only class types and other newtypes are allowed in the base.

---

_Merged by @oconnor663 on 2026-01-06 17:32_

---

_Closed by @oconnor663 on 2026-01-06 17:32_

---

_Branch deleted on 2026-01-06 17:32_

---
