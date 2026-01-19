```yaml
number: 22627
title: "[ty] Support deferred and recursive type evaluation in `NamedTuple(...)`"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: charlie/defer
created_at: 2026-01-16T18:27:15Z
updated_at: 2026-01-19T12:28:17Z
url: https://github.com/astral-sh/ruff/pull/22627
synced_at: 2026-01-19T13:34:31Z
```

# [ty] Support deferred and recursive type evaluation in `NamedTuple(...)`

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/2528.

Closes https://github.com/astral-sh/ty/issues/2529.


---

_Label `ty` added by @charliermarsh on 2026-01-16 18:27_

---

_Comment by @astral-sh-bot[bot] on 2026-01-16 18:29_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-16 18:30_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 46 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Yarn[Any] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- Found 1825 diagnostics
+ Found 1823 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-16 19:40_

---

_Review requested from @carljm by @charliermarsh on 2026-01-16 19:40_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-16 19:40_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-16 19:40_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-16 19:40_

---

_Label `ecosystem-analyzer` added by @charliermarsh on 2026-01-16 19:58_

---

_Comment by @astral-sh-bot[bot] on 2026-01-16 20:04_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 0 | 3 | 6 |
| `invalid-parameter-default` | 0 | 0 | 7 |
| `invalid-argument-type` | 0 | 1 | 5 |
| `invalid-assignment` | 0 | 0 | 5 |
| `possibly-missing-attribute` | 0 | 3 | 1 |
| `invalid-await` | 0 | 2 | 0 |
| `unresolved-attribute` | 0 | 0 | 2 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **1** | **9** | **26** |


**[Full report with detailed diff](https://1ce19951.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://1ce19951.ty-ecosystem-ext.pages.dev/timing))



---

_Assigned to @AlexWaygood by @AlexWaygood on 2026-01-18 23:51_

---

_Comment by @AlexWaygood on 2026-01-19 12:26_

Thanks @charliermarsh ❤️

There's a lot to like about this PR... but there's also quite a few things I'd do differently. In particular, I don't think it's necessary to defer inference of `NamedTuple` specs for "dangling" calls -- while it's true that something like this can have a recursive reference in it:

```py
from typing import NamedTuple

class Foo(NamedTuple("F", [("x", "Foo | None")]):
    pass
```

... that's already dealt with by our existing logic on `main` that defers _everything_ inside a class's bases list:

https://github.com/astral-sh/ruff/blob/55a174ed95c757a88199fdc1b15e51fd9041b4fa/crates/ty_python_semantic/src/types/infer/builder.rs#L3191-L3201

I've taken a stab at solving this problem myself in https://github.com/astral-sh/ruff/pull/22718, though I want to pull some parts of that PR into standalone changes, since there's some refactoring in there that makes the overall diff a bit confusing at the moment. Overall it ends up being a slightly "messier" diff (there's more code being removed/rewritten rather than simply being added) but it ends up being quite a few LOC less than this PR overall.

One problem that both our PRs have, however, is that they both currently have stack overflows on this test case:

```py
from typing import NamedTuple

A = NamedTuple("A", [("x", "B | None")])
B = NamedTuple("B", [("x", "C")])
C = NamedTuple("C", [("x", A)])

A(x=C())
```

(your PR only seems to overflow on that snippet after `main` has been merged in, which is interesting)

I'm not _yet_ sure how to fix that...

---

_Comment by @AlexWaygood on 2026-01-19 12:28_

(I did just copy-and-paste some of your tests and code straight from this PR into my PR, so I'll definitely list you as a co-author!)

---
