```yaml
number: 21704
title: "[ty] Emit a diagnostic for subclassing with `order=True`"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: charlie/order
created_at: 2025-11-30T16:12:08Z
updated_at: 2026-01-07T01:01:56Z
url: https://github.com/astral-sh/ruff/pull/21704
synced_at: 2026-01-10T16:30:32Z
```

# [ty] Emit a diagnostic for subclassing with `order=True`

---

_Pull request opened by @charliermarsh on 2025-11-30 16:12_

## Summary

Closes https://github.com/astral-sh/ty/issues/1681.


---

_Review requested from @carljm by @charliermarsh on 2025-11-30 16:12_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-11-30 16:12_

---

_Review requested from @sharkdp by @charliermarsh on 2025-11-30 16:12_

---

_Review requested from @dcreager by @charliermarsh on 2025-11-30 16:12_

---

_Review requested from @MichaReiser by @charliermarsh on 2025-11-30 16:12_

---

_Renamed from "Emit a diagnostic for subclassing with `order=True`" to "[ty] Emit a diagnostic for subclassing with `order=True`" by @charliermarsh on 2025-11-30 16:12_

---

_Label `ty` added by @charliermarsh on 2025-11-30 16:12_

---

_Comment by @astral-sh-bot[bot] on 2025-11-30 16:14_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-30 16:15_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
+ tests/test_init_subclass.py:22:19: warning[subclass-of-dataclass-with-order] Class `Vanilla` inherits from dataclass `Base` which has `order=True`
+ tests/test_make.py:264:17: warning[subclass-of-dataclass-with-order] Class `C` inherits from dataclass `B` which has `order=True`
+ tests/test_make.py:1138:17: warning[subclass-of-dataclass-with-order] Class `C` inherits from dataclass `Base` which has `order=True`
+ tests/test_setattr.py:323:17: warning[subclass-of-dataclass-with-order] Class `B` inherits from dataclass `A` which has `order=True`
- Found 616 diagnostics
+ Found 620 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

Expression (https://github.com/cognitedata/Expression)
+ expression/core/try_.py:17:11: warning[subclass-of-dataclass-with-order] Class `Try` inherits from dataclass `Result` which has `order=True`
- Found 228 diagnostics
+ Found 229 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
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
- Found 5528 diagnostics
+ Found 5533 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2805 diagnostics
+ Found 2808 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5154 diagnostics
+ Found 5156 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
+ misc/python/materialize/output_consistency/ignore_filter/ignore_verdict.py:18:17: warning[subclass-of-dataclass-with-order] Class `YesIgnore` inherits from dataclass `IgnoreVerdict` which has `order=True`
- Found 311 diagnostics
+ Found 312 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @charliermarsh on 2025-11-30 16:18_

I believe those changes on the conformance tests are "correct" in-so-far as those are instances of the case we want to catch here.

---

_Label `ecosystem-analyzer` added by @MichaReiser on 2025-11-30 16:29_

---

_Comment by @astral-sh-bot[bot] on 2025-11-30 16:38_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 0 | 1 | 10 |
| `invalid-argument-type` | 2 | 0 | 4 |
| `invalid-assignment` | 0 | 0 | 5 |
| `subclass-of-dataclass-with-order` | 5 | 0 | 0 |
| **Total** | **7** | **1** | **19** |


**[Full report with detailed diff](https://4c164f80.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://4c164f80.ty-ecosystem-ext.pages.dev/timing))



---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclasses.md`:354 on 2025-12-01 18:54_

```suggestion
classes in the inheritance hierarchy will raise a `TypeError` at runtime. The design of the stdlib
feature therefore violates the Liskov Substitution Principle:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/liskov.md`:489 on 2025-12-01 18:57_

I wonder if we should suppress the `subclass-of-dataclass-with-order` diagnostic if all the comparison methods are overridden on the subclass? (That's the case here, since `Bar2` is also decorated with `order=True`.) This case here _does_ feel like it should be flagged using the error code we use for Liskov Subsitution Principle violations, because by specifying `order=True` the user has in a sense explicitly asked for an incompatible override.

And something like the following also seems safe, if the user has taken care to override all the unsound methods from the superclass:

```py
from dataclasses import dataclass

@dataclass(order=True)
class A:
    int

class B(A):
    def __lt__(self, other: A) -> bool:
        return True

    def __le__(self, other: A) -> bool:
        return True

    __gt__ = __lt__
    __ge__ = __le__
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:818 on 2025-12-01 19:01_

can we add an `info` subdiagnostic here explaining why this is problematic?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1669 on 2025-12-01 19:01_

```suggestion
    /// class Child(Parent):  # Ty emits an error here
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1678 on 2025-12-01 19:02_

```suggestion
    /// Consider using [`functools.total_ordering`][total_ordering] instead, which does not have this limitation.
    ///
    /// [Liskov Substitution Principle]: https://en.wikipedia.org/wiki/Liskov_substitution_principle
    /// [total_ordering]: https://docs.python.org/3/library/functools.html#functools.total_ordering
```

---

_@AlexWaygood reviewed on 2025-12-01 19:02_

Nice!

---

_@MichaReiser reviewed on 2025-12-01 21:11_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/diagnostic.rs`:1669 on 2025-12-01 21:11_

...ty

---

_Comment by @AlexWaygood on 2026-01-06 15:32_

@charliermarsh are you still interested in getting this over the line?

---

_Comment by @charliermarsh on 2026-01-06 15:35_

Yes sir, sorry, lost track.

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-07 00:55_

---

_Comment by @charliermarsh on 2026-01-07 01:01_

I think the Expression diagnostic is correct:

```
  Expression (Try inherits from Result)

  - Result is decorated with @tagged_union(frozen=True, order=True)
  - tagged_union is marked with @dataclass_transform(), so ty recognizes it as a dataclass-like decorator
  - When order=True is passed, the class gets ordering methods
  - Try inherits from Result without defining its own comparison methods
  - True positive: At runtime, Try(Success(1)) < Result(Success(2)) would raise TypeError
```

---
