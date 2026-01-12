```yaml
number: 22011
title: "[ty] Don't use implicit superclass annotation when converting a class constructor into a `Callable`"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dcreager/fix-class-callable
created_at: 2025-12-16T18:14:30Z
updated_at: 2025-12-16T18:37:13Z
url: https://github.com/astral-sh/ruff/pull/22011
synced_at: 2026-01-12T15:57:39Z
```

# [ty] Don't use implicit superclass annotation when converting a class constructor into a `Callable`

---

_@dcreager_

This fixes a bug @zsol found running ty against pyx. His original repro is:

```py
class Base:
    def __init__(self) -> None: pass

class A(Base):
    pass

def foo[T](callable: Callable[..., T]) -> T:
    return callable()

a: A = foo(A)
```

The call at the bottom would fail, since we would infer `() -> Base` as the callable type of `A`, when it should be `() -> A`.

The issue was how we add implicit annotations to `self` parameters. Typically, we turn it into `self: Self`. But in cases where we don't need to introduce a full typevar, we turn it into `self: [the class itself]` — in this case, `self: Base`. Then, when turning the class constructor into a callable, we would see this non-`Self` annotation and think that it was important and load-bearing.

The fix is that we skip all implicit annotations when determining whether the `self` annotation should take precedence in the callable's return type.

---

_Review requested from @carljm by @dcreager on 2025-12-16 18:14_

---

_Review requested from @AlexWaygood by @dcreager on 2025-12-16 18:14_

---

_Label `ty` added by @dcreager on 2025-12-16 18:14_

---

_Review requested from @sharkdp by @dcreager on 2025-12-16 18:14_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-12-16 18:14_

---

_Comment by @astral-sh-bot[bot] on 2025-12-16 18:16_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_@carljm approved on 2025-12-16 18:16_

Looks good!

---

_Comment by @astral-sh-bot[bot] on 2025-12-16 18:21_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
artigraph (https://github.com/artigraph/artigraph)
- tests/arti/graphs/test_graph.py:450:20: error[unresolved-attribute] Object of type `CallableMixin` has no attribute `called`
- tests/arti/graphs/test_graph.py:452:16: error[unresolved-attribute] Object of type `CallableMixin` has no attribute `called`
- Found 151 diagnostics
+ Found 149 diagnostics

optuna (https://github.com/optuna/optuna)
- tests/samplers_tests/tpe_tests/test_multi_objective_sampler.py:44:9: error[unresolved-attribute] Unresolved attribute `return_value` on type `CallableMixin`.
- tests/samplers_tests/tpe_tests/test_multi_objective_sampler.py:45:9: error[unresolved-attribute] Unresolved attribute `return_value` on type `CallableMixin`.
- tests/samplers_tests/tpe_tests/test_multi_objective_sampler.py:113:13: error[unresolved-attribute] Unresolved attribute `return_value` on type `CallableMixin`.
- tests/samplers_tests/tpe_tests/test_multi_objective_sampler.py:114:13: error[unresolved-attribute] Unresolved attribute `return_value` on type `CallableMixin`.
- Found 552 diagnostics
+ Found 548 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/coding/cftime_offsets.py:687:54: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | type[BaseCFTimeOffset] | partial[QuarterOffset]]` is not assignable to `Mapping[str, type[BaseCFTimeOffset]]`
+ xarray/coding/cftime_offsets.py:687:54: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | type[BaseCFTimeOffset] | partial[QuarterBegin] | partial[QuarterEnd]]` is not assignable to `Mapping[str, type[BaseCFTimeOffset]]`
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
- xarray/core/dataset.py:8875:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataset.py:8875:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/i18n/__init__.py:188:12: error[unresolved-attribute] Object of type `PurePath` has no attribute `is_file`
- Found 1142 diagnostics
+ Found 1141 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 7 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 7 union elements, object_ | Self@iloc]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any], generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`

jax (https://github.com/google/jax)
+ jax/_src/tree_util.py:295:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:298:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:301:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2797 diagnostics
+ Found 2800 diagnostics

zulip (https://github.com/zulip/zulip)
- zerver/tests/test_external.py:260:13: error[unresolved-attribute] Unresolved attribute `return_value` on type `CallableMixin`.
- Found 3669 diagnostics
+ Found 3668 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/test_groupby.py:439:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(str & Any) | (bytes & Any) | (int & Any) | ... omitted 12 union elements]`
+ tests/test_groupby.py:439:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(Any & str) | (Any & bytes) | (Any & int) | ... omitted 12 union elements]`
- tests/test_resampler.py:402:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(str & Any) | (bytes & Any) | (int & Any) | ... omitted 12 union elements]`
+ tests/test_resampler.py:402:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(Any & str) | (Any & bytes) | (Any & int) | ... omitted 12 union elements]`

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14401 diagnostics
+ Found 14402 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-16 18:23_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 0 | 8 | 0 |
| `invalid-return-type` | 1 | 0 | 6 |
| `invalid-assignment` | 0 | 0 | 6 |
| `invalid-argument-type` | 0 | 0 | 3 |
| **Total** | **1** | **8** | **15** |

**[Full report with detailed diff](https://dcreager-fix-class-callable.ecosystem-663.pages.dev/diff)** ([timing results](https://dcreager-fix-class-callable.ecosystem-663.pages.dev/timing))




---

_Merged by @dcreager on 2025-12-16 18:37_

---

_Closed by @dcreager on 2025-12-16 18:37_

---

_Branch deleted on 2025-12-16 18:37_

---
