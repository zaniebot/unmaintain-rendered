```yaml
number: 22085
title: "[ty] Fix bug in union builder optimization"
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: micha/union-builder-sub-unions
head: zb/fix-sub-union
created_at: 2025-12-19T14:56:54Z
updated_at: 2026-01-18T14:31:19Z
url: https://github.com/astral-sh/ruff/pull/22085
synced_at: 2026-01-18T15:23:26Z
```

# [ty] Fix bug in union builder optimization

---

_@zanieb_

Just poking around...

Applied to https://github.com/astral-sh/ruff/pull/22071

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 14:58_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-19 14:59_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

jax (https://github.com/google/jax)
+ jax/_src/tree_util.py:295:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:298:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:301:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2799 diagnostics
+ Found 2802 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Index[Any]] | Top[Series[Any, Any]] | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 7 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`


```

</details>


No memory usage changes detected ✅



---

_Closed by @zanieb on 2025-12-19 15:02_

---

_Comment by @zanieb on 2025-12-19 15:02_

This looks wrong, I'll keep poking in the background.

---

_Comment by @MichaReiser on 2025-12-19 15:03_

It looks "correct" in the sense that it fixes the bug in my PR

---

_Comment by @zanieb on 2025-12-19 15:08_

Oh does it? It looked like it caused some new problems too :)

---

_Reopened by @zanieb on 2025-12-19 15:08_

---

_Comment by @MichaReiser on 2025-12-19 15:10_

> Oh does it? It looked like it caused some new problems too :)

It does. So I think you might be on to something but I honestly don't know enough about those union builder flags (or when we should set which flag)

---

_Comment by @mtshiba on 2025-12-19 15:30_

`UnionBuilder::cycle_recovery` is a flag required due to the technical constraint that query cycles cannot occur within cycle recovery functions. When this is `true`, a cheap union operation is performed without using `is_redundant_with`. This is fine for use within cycle normalization.

`UnionBuilder::recursively_defined` was introduced in #21683. Setting it to `Yes` will perform widening earlier, which will result in faster fixed-point convergence. The rule for `recursively_defined` is that this attribute is infectious, i.e. if you find a sub-union with `recursively_defined = Yes`, you should also mark the final union type as `recursively_defined = Yes`.
If you are seeing a too-many-cycle panic due to unbounded growth of the literal type, you probably have `recursively_defined` set to `No` when it should be `Yes`.

There is no direct relationship between the two. `recursively_defined = Yes` should be annotated for union types of recursively defined expressions. Specifically, expressions with a type like `T | Divergent` are considered recursively defined (ref. https://github.com/astral-sh/ruff/blob/9809405b053280e3f0e47bb5c762108e70a15519/crates/ty_python_semantic/src/types.rs#L14075-L14080).


---

_Comment by @zanieb on 2025-12-19 15:47_

@MichaReiser CI is green now 🤔 

---

_Comment by @MichaReiser on 2025-12-19 16:16_

> @MichaReiser CI is green now 🤔

Unfortunately, it doesn't remove the prefect diagnostics anymore.

---

_Closed by @zanieb on 2026-01-18 14:31_

---
