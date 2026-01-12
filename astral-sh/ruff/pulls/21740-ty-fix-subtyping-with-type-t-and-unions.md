```yaml
number: 21740
title: "[ty] Fix subtyping with `type[T]` and unions"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
assignees: []
merged: true
base: main
head: ibraheem/type-of-subtyping
created_at: 2025-12-01T21:06:21Z
updated_at: 2025-12-01T23:20:32Z
url: https://github.com/astral-sh/ruff/pull/21740
synced_at: 2026-01-12T15:57:32Z
```

# [ty] Fix subtyping with `type[T]` and unions

---

_@ibraheemdev_

## Summary

Resolves https://github.com/astral-sh/ruff/pull/21685#issuecomment-3591695954.

---

_Review requested from @carljm by @ibraheemdev on 2025-12-01 21:06_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-12-01 21:06_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-12-01 21:06_

---

_Review requested from @dcreager by @ibraheemdev on 2025-12-01 21:06_

---

_Label `ty` added by @ibraheemdev on 2025-12-01 21:06_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 21:08_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-01 21:10_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 498 diagnostics
+ Found 496 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/vendor/jsonschema/exceptions.py:240:26: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 7952 diagnostics
+ Found 7951 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/testing/_raises_group.py:811:32: error[not-iterable] Object of type `enumerate[type[BaseExcT_co@RaisesGroup] | Matcher[BaseExcT_co@RaisesGroup] | RaisesGroup[BaseException]]` is not iterable
- src/trio/testing/_raises_group.py:834:32: error[not-iterable] Object of type `enumerate[type[BaseExcT_co@RaisesGroup] | Matcher[BaseExcT_co@RaisesGroup] | RaisesGroup[BaseException]]` is not iterable
- src/trio/testing/_raises_group.py:895:36: error[not-iterable] Object of type `enumerate[type[BaseExcT_co@RaisesGroup] | Matcher[BaseExcT_co@RaisesGroup] | RaisesGroup[BaseException]]` is not iterable
- Found 493 diagnostics
+ Found 490 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/flow_runs.py:291:19: error[unresolved-attribute] Object of type `type[T@_in_process_pause]` has no attribute `load`
+ src/prefect/flow_runs.py:291:19: error[unresolved-attribute] Object of type `(type[T@_in_process_pause] & ~AlwaysTruthy & ~AlwaysFalsy) | (type[T@_in_process_pause] & ~AlwaysFalsy)` has no attribute `load`
- src/prefect/flow_runs.py:306:15: error[unresolved-attribute] Object of type `type[T@_in_process_pause]` has no attribute `save`
+ src/prefect/flow_runs.py:306:15: error[unresolved-attribute] Object of type `(type[T@_in_process_pause] & ~AlwaysTruthy & ~AlwaysFalsy) | (type[T@_in_process_pause] & ~AlwaysFalsy)` has no attribute `save`
- src/prefect/flow_runs.py:318:34: error[unresolved-attribute] Object of type `type[T@_in_process_pause]` has no attribute `load`
+ src/prefect/flow_runs.py:318:34: error[unresolved-attribute] Object of type `(type[T@_in_process_pause] & ~AlwaysTruthy & ~AlwaysFalsy) | (type[T@_in_process_pause] & ~AlwaysFalsy)` has no attribute `load`
- src/prefect/flow_runs.py:445:26: error[unresolved-attribute] Object of type `type[T@suspend_flow_run]` has no attribute `load`
+ src/prefect/flow_runs.py:445:26: error[unresolved-attribute] Object of type `(type[T@suspend_flow_run] & ~AlwaysTruthy & ~AlwaysFalsy) | (type[T@suspend_flow_run] & ~AlwaysFalsy)` has no attribute `load`
- src/prefect/flow_runs.py:455:15: error[unresolved-attribute] Object of type `type[T@suspend_flow_run]` has no attribute `save`
+ src/prefect/flow_runs.py:455:15: error[unresolved-attribute] Object of type `(type[T@suspend_flow_run] & ~AlwaysTruthy & ~AlwaysFalsy) | (type[T@suspend_flow_run] & ~AlwaysFalsy)` has no attribute `save`
- src/prefect/utilities/collections.py:197:40: error[invalid-argument-type] Argument to function `ensure_iterable` is incorrect: Expected `tuple[type[T@extract_instances], ...] | Iterable[tuple[type[T@extract_instances], ...]]`, found `type[T@extract_instances] | tuple[type[T@extract_instances], ...]`
- src/prefect/utilities/collections.py:210:16: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[type[T@extract_instances], list[Any]].__getitem__(key: type[T@extract_instances], /) -> list[Any]` cannot be called with key of type `tuple[type[T@extract_instances], ...]` on object of type `dict[type[T@extract_instances], list[Any]]`
+ src/prefect/utilities/collections.py:210:16: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[type[T@extract_instances], list[Any]].__getitem__(key: type[T@extract_instances], /) -> list[Any]` cannot be called with key of type `Unknown` on object of type `dict[type[T@extract_instances], list[Any]]`
- src/prefect/utilities/pydantic.py:85:9: error[invalid-assignment] Implicit shadowing of function `__reduce__`
+ src/prefect/utilities/pydantic.py:85:9: error[invalid-assignment] Object of type `def _reduce_model(self: BaseModel) -> tuple[Any, ...]` is not assignable to attribute `__reduce__` on type `type[M@add_cloudpickle_reduction] & ~AlwaysFalsy`
- Found 3400 diagnostics
+ Found 3399 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 41 diagnostics
+ Found 42 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/__init__.py:1019:9: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Argument type `list[Unknown | type[Self@db_identity]]` does not satisfy upper bound `list[_T@list]` of type variable `Self`
- ibis/backends/__init__.py:1020:9: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Argument type `list[Unknown | type[Self@db_identity]]` does not satisfy upper bound `list[_T@list]` of type variable `Self`
- Found 4489 diagnostics
+ Found 4487 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/core/exceptiontools.py:202:68: error[not-iterable] Object of type `(Unknown & ~AlwaysFalsy) | (tuple[type[T@getattr_], ...] & ~AlwaysFalsy) | (tuple[type[@Todo], ...] & ~AlwaysFalsy)` may not be iterable
- hydpy/core/variabletools.py:2345:18: error[not-iterable] Object of type `Iterable[type[TypeVariable@sort_variables] | tuple[type[TypeVariable@sort_variables], T@sort_variables]]` is not iterable
- Found 654 diagnostics
+ Found 652 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/core/basic.py:670:76: error[not-iterable] Object of type `tuple[Tbasic@atoms | type[Tbasic@atoms], ...] & ~AlwaysFalsy` is not iterable
+ sympy/matrices/matrixbase.py:1592:20: error[invalid-return-type] Return type does not match returned value: expected `set[Basic] | set[Tbasic@atoms]`, found `set[type[Tbasic@atoms]]`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_@AlexWaygood approved on 2025-12-01 22:05_

---

_Merged by @ibraheemdev on 2025-12-01 23:20_

---

_Closed by @ibraheemdev on 2025-12-01 23:20_

---

_Branch deleted on 2025-12-01 23:20_

---
