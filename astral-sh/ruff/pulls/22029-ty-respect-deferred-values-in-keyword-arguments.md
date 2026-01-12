```yaml
number: 22029
title: "[ty] Respect deferred values in keyword arguments et al for `.pyi` files"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/defer
created_at: 2025-12-17T16:32:02Z
updated_at: 2025-12-17T22:02:13Z
url: https://github.com/astral-sh/ruff/pull/22029
synced_at: 2026-01-12T15:57:39Z
```

# [ty] Respect deferred values in keyword arguments et al for `.pyi` files

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/2019.


---

_Label `ty` added by @charliermarsh on 2025-12-17 16:32_

---

_Comment by @astral-sh-bot[bot] on 2025-12-17 16:35_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-17 16:36_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8875:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8875:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 44 diagnostics

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:295:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:298:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:301:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2794 diagnostics
+ Found 2791 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Index[Any]] | Top[Series[Any, Any]] | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 7 union elements, object_]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1218:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5081 diagnostics
+ Found 5080 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14391 diagnostics
+ Found 14392 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @AlexWaygood on 2025-12-17 16:44_

if we make any changes here, we might just want to consider deferring _all_ lookups in stub files rather than carving out more special cases (I think that's what pyright does, and I think mypy does a variation of it). We've held off on doing that until now because it'll probably slow us down, though, and we weren't sure what the use case is other than that other type checkers allow arbitrary forward references

---

_Comment by @AlexWaygood on 2025-12-17 16:49_

some previous discussion in https://github.com/astral-sh/ty/issues/394#issuecomment-3591856279

---

_Marked ready for review by @charliermarsh on 2025-12-17 17:00_

---

_Review requested from @carljm by @charliermarsh on 2025-12-17 17:00_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-17 17:00_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-17 17:00_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-17 17:00_

---

_Comment by @charliermarsh on 2025-12-17 17:00_

> if we make any changes here, we might just want to consider deferring all lookups in stub files rather than carving out more special cases (I think that's what pyright does, and I think mypy does a variation of it).

I feel like the behavior here is a clear improvement though -- are there major downsides? This seems similar to what Ruff does.


---

_Comment by @AlexWaygood on 2025-12-17 18:16_

I think the major downside is that ty's behaviour becomes harder to explain to users and reason about. I think the user confusion shown in the issue is pretty good motivation for changing our behaviour -- our behaviour is currently different to pyright's, for no clear reason other than that we're worried it might be a bit slower. But this doesn't really fix that underlying issue -- we still don't match pyright's behaviour with this change. And with this change, there's no longer really any rhyme or reason to which expressions we use deferred lookup for in stubs and which ones we don't.

If we just use deferred lookup everywhere for stub files, that will match pyright's behaviour, would simplify our code a fair bit, and would be a consistent rule that would be easy to explain to users

---

_Comment by @charliermarsh on 2025-12-17 18:36_

I put up an alternative here: https://github.com/astral-sh/ruff/pull/22034. I haven't checked if this is actually true, but personally I'd be comfortable with "has the same behavior as Ruff" for these deferrals, since that behavior is also very proven.

---

_Comment by @AlexWaygood on 2025-12-17 19:04_

(curious for @carljm's thoughts here too, especially since it looks like https://github.com/astral-sh/ruff/pull/22034 does indeed make us more likely to run into cycles...)

---

_Comment by @carljm on 2025-12-17 21:55_

I'm in favor of going ahead with this. Default values are in a gray area where they kinda don't belong in a stub file in principle, since they are irrelevant to typing -- but it also can make a lot of sense to put them there for IDE / documentation purposes. And I think there's no strong reason not to defer name resolution in them.

I think in practice this PR moves us pretty close to just deferring everywhere (in real-world stub files), because by now we defer almost all cases you are actually likely to see in a stub. So in practice I don't suspect it will cause much confusion.

---

_Comment by @carljm on 2025-12-17 21:58_

I think #22034 is probably the right idea in principle, but it means we'd either need to start ignoring more stuff as a special case in stub files that's irrelevant there, change our `linter_no_panic` tests, or track down some new cycle issues. And none of those seem like high priority things to do right now.

---

_Comment by @AlexWaygood on 2025-12-17 21:59_

Yeah, I still prefer the _idea_ of #22034 but given the panics on that one I'm okay going with this for the time being.

---

_Comment by @charliermarsh on 2025-12-17 22:00_

I think I’m in the same boat.

---

_@carljm approved on 2025-12-17 22:00_

---

_Merged by @carljm on 2025-12-17 22:02_

---

_Closed by @carljm on 2025-12-17 22:02_

---

_Branch deleted on 2025-12-17 22:02_

---
