```yaml
number: 22419
title: "Allow Python 3.15 as valid `target-version` value in preview"
type: pull_request
state: merged
author: Jkhall81
labels:
  - configuration
  - python315
assignees: []
merged: true
base: main
head: fix/ruf017-pep798
created_at: 2026-01-06T16:04:50Z
updated_at: 2026-01-07T08:38:37Z
url: https://github.com/astral-sh/ruff/pull/22419
synced_at: 2026-01-10T16:30:32Z
```

# Allow Python 3.15 as valid `target-version` value in preview

---

_Pull request opened by @Jkhall81 on 2026-01-06 16:04_

## Summary
Adds support for Python 3.15 to the Ruff workspace, gating it behind the `preview` flag with an unstable version warning as requested.

Part of #22230

---

_Review requested from @carljm by @Jkhall81 on 2026-01-06 16:04_

---

_Review requested from @MichaReiser by @Jkhall81 on 2026-01-06 16:04_

---

_Review requested from @sharkdp by @Jkhall81 on 2026-01-06 16:04_

---

_Review requested from @dcreager by @Jkhall81 on 2026-01-06 16:04_

---

_Comment by @astral-sh-bot[bot] on 2026-01-06 16:06_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-06 16:07_


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
- xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
- xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`

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

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2808 diagnostics
+ Found 2805 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5155 diagnostics
+ Found 5156 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14474 diagnostics
+ Found 14475 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@MichaReiser reviewed on 2026-01-06 16:12_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/python_version.rs`:76 on 2026-01-06 16:12_

We should not change `latest` or `latest_ty` before we have stable Python 3.15 support

---

_Comment by @astral-sh-bot[bot] on 2026-01-06 16:17_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Label `python315` added by @amyreese on 2026-01-06 16:19_

---

_Comment by @amyreese on 2026-01-06 16:22_

> Rule Update (RUF017): Implemented a version gate for quadratic-list-summation. Per PEP 798, sum() is now efficient for list-of-lists in Python 3.15, making this lint redundant for newer targets.

Is this just a byproduct of the changes from that PEP? I don't see anything about `sum()` mentioned in it.

---

_@Jkhall81 reviewed on 2026-01-06 16:38_

---

_Review comment by @Jkhall81 on `crates/ruff_python_ast/src/python_version.rs`:76 on 2026-01-06 16:38_

`latest` and `latest_ty` set back to 314.

---

_Comment by @amyreese on 2026-01-06 16:50_

Perhaps we should split out the RUF017 changes to a separate PR? And maybe find better toy examples that demonstrate the issue? On my Mac, it actually shows the *opposite* result for me, with `sum()` being faster than `reduce()` on 3.10 and 3.12, and roughly equivalent on 3.14 and 3.15... 🤔 

3.10:
```
amethyst@lunatone ~ » uvx python3.10 -m timeit -s 'import operator, functools; data = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]' 'sum(data, [])'
5000000 loops, best of 5: 72.6 nsec per loop

amethyst@lunatone ~ » uvx python3.10 -m timeit -s 'import operator, functools; data = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]' 'functools.reduce(operator.iadd, data, [])'
5000000 loops, best of 5: 85.1 nsec per loop
```
3.12:
```
amethyst@lunatone ~ » uvx python3.12 -m timeit -s 'import operator, functools; data = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]' 'sum(data, [])'
5000000 loops, best of 5: 62.8 nsec per loop

amethyst@lunatone ~ » uvx python3.12 -m timeit -s 'import operator, functools; data = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]' 'functools.reduce(operator.iadd, data, [])'
5000000 loops, best of 5: 84.4 nsec per loop
```
3.14:
```
amethyst@lunatone ~ » uvx python3.14 -m timeit -s 'import operator, functools; data = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]' 'sum(data, [])'
5000000 loops, best of 5: 73.5 nsec per loop

amethyst@lunatone ~ » uvx python3.14 -m timeit -s 'import operator, functools; data = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]' 'functools.reduce(operator.iadd, data, [])'
5000000 loops, best of 5: 75.3 nsec per loop
```


---

_Comment by @dylwil3 on 2026-01-06 17:20_

> On my Mac, it actually shows the opposite result for me

Maybe the list of lists was too small to see the difference?

```console
❯ uvx python3.12 -m timeit -s 'import operator, functools; data = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]*100' 'sum(data, [])'
2000 loops, best of 5: 119 usec per loop

❯ uvx python3.12 -m timeit -s 'import operator, functools; data = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]*100' 'functools.reduce(operator.iadd, data, [])'
50000 loops, best of 5: 5.44 usec per loop
```

---

_Comment by @dylwil3 on 2026-01-06 17:40_

> > Rule Update (RUF017): Implemented a version gate for quadratic-list-summation. Per PEP 798, sum() is now efficient for list-of-lists in Python 3.15, making this lint redundant for newer targets.
> 
> Is this just a byproduct of the changes from that PEP? I don't see anything about `sum()` mentioned in it.

I'm also confused. Neither the PEP in question nor its (unmerged) [implementation](https://github.com/python/cpython/pull/143056) touches anything to do with `sum`, right? Am I missing something?

Here's what I get building CPython with configurations `./configure --enable-optimizations --with-lto` off the branch implementing PEP 798:

```console
❯ ./python.exe --version
Python 3.15.0a3+

❯ ./python.exe -m timeit -s 'import operator, functools; data = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]*100' 'sum(data, [])'
2000 loops, best of 5: 124 usec per loop

❯ ./python.exe -m timeit -s 'import operator, functools; data = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]*100' 'functools.reduce(operator.iadd, data, [])'
50000 loops, best of 5: 6.04 usec per loop

❯ ./python.exe -m timeit -s 'import operator, functools; data = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]*100' '[x for l in data for x in l]'
50000 loops, best of 5: 7.55 usec per loop

❯ ./python.exe -m timeit -s 'import operator, functools; data = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]*100' '[*x for x in data]'
50000 loops, best of 5: 4.82 usec per loop
```

So it doesn't look like `sum` is magically fast now. On the other hand, the PEP 798 syntax is now fastest!

Don't we want to keep the rule but just change the autofix? 

---

_Comment by @Jkhall81 on 2026-01-06 17:51_

Thanks for the benchmarks, @dylwil3.  Looks like I misinterpreted the impact of PEP 798 on `sum()`.

Just to confirm my understanding is correct for the next steps:
1. Remove the version gate so RUF017 stays active for 3.15.
2. Update the suggested fix for 3.15+ targets to use the new `[*x for x in data]` syntax instead of `reduce`.

Does that cover everything?

---

_Comment by @amyreese on 2026-01-06 17:55_

Let's split the changes to RUF017 into a separate PR, and leave this one as just adding support for 3.15.

---

_Comment by @dylwil3 on 2026-01-06 17:59_

Agreed. And could you maybe add something to `preview.rs` and emit a warning when preview is not enabled, as was done for 3.14 support before it was stabilized? See https://github.com/astral-sh/ruff/pull/17647 for prior art

---

_Review request for @dcreager removed by @AlexWaygood on 2026-01-06 20:40_

---

_Review request for @carljm removed by @AlexWaygood on 2026-01-06 20:40_

---

_Review request for @sharkdp removed by @AlexWaygood on 2026-01-06 20:40_

---

_@amyreese approved on 2026-01-06 21:05_

---

_Review requested from @ntBre by @amyreese on 2026-01-06 21:05_

---

_Renamed from "feat(ruff): add Python 3.15 support and retire RUF017" to "feat(ruff): add Python 3.15 support" by @amyreese on 2026-01-06 21:05_

---

_Label `configuration` added by @dylwil3 on 2026-01-06 23:56_

---

_Renamed from "feat(ruff): add Python 3.15 support" to "Allow Python 3.15 as valid `target-version` value in preview" by @MichaReiser on 2026-01-07 08:37_

---

_@MichaReiser approved on 2026-01-07 08:38_

---

_Merged by @MichaReiser on 2026-01-07 08:38_

---

_Closed by @MichaReiser on 2026-01-07 08:38_

---
