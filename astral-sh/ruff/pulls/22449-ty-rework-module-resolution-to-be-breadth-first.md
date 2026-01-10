```yaml
number: 22449
title: "[ty] Rework module resolution to be breadth-first instead of depth-first"
type: pull_request
state: open
author: Gankra
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: gankra/namespace-auto
created_at: 2026-01-08T01:47:20Z
updated_at: 2026-01-09T18:08:02Z
url: https://github.com/astral-sh/ruff/pull/22449
synced_at: 2026-01-10T15:56:07Z
```

# [ty] Rework module resolution to be breadth-first instead of depth-first

---

_Pull request opened by @Gankra on 2026-01-08 01:47_

## Summary

By making the algorithm breadth-first/incremental, the logic has a coherent translation to computing all_modules. Thus this is ground-work for auto-complete and auto-import adding various missing import semantics (extremely optimistically, we could actually make it share the same code!).

In addition, this fixes two corner-case issues with our module resolution:

* A regular package (or module) in a later search-path now properly shadows a namespace package in an earlier one, which matches runtime behaviour aiui.
* A regular package in a later search-path now properly(?) shadows a module  in an earlier one (previously we treated `foo.py` similarly to a legacy namespace package where `import foo` would find it but `import foo.bar` would ignore it (and find any regular-package or namespace-package `foo` on subsequent search-paths).
* ~~We now consider all stub-packages to have higher priority than non-stub-packages, independent of search-path ordering. In most cases this means our behaviour will now be "search all paths for stubs, then search all paths for implementations" -- this isn't strictly true in this current implementation because two namespace packages could exist where one specifies a.pyi and the other specifies a.py and we still respect search-path order in that case (I think the impl is actually really close to fixing that but I finally got the tests passing and I didn't want to poke the bear).~~

## Test Plan

I need to write more tests to cover interesting cases I've thought of, for now I'm content with the existing tests passing (a few have been changed for now, I think all those changes are acceptable, but ymmv).


---

_Label `ty` added by @Gankra on 2026-01-08 01:47_

---

_Label `ecosystem-analyzer` added by @Gankra on 2026-01-08 01:47_

---

_Comment by @astral-sh-bot[bot] on 2026-01-08 01:49_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-08 01:50_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
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

schema_salad (https://github.com/common-workflow-language/schema_salad)
+ schema_salad/python_codegen.py:62:17: error[unknown-argument] Argument `target_versions` does not match any known parameter of bound method `__init__`
+ schema_salad/python_codegen.py:62:66: error[unknown-argument] Argument `line_length` does not match any known parameter of bound method `__init__`
- Found 249 diagnostics
+ Found 251 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

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

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- Found 1838 diagnostics
+ Found 1837 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     struct fields = ~3MB
+     struct fields = ~4MB

trio (https://github.com/python-trio/trio)
-     struct fields = ~11MB
+     struct fields = ~12MB


```

</details>




---

_Comment by @astral-sh-bot[bot] on 2026-01-08 01:52_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 1 | 0 | 7 |
| `unknown-argument` | 2 | 0 | 0 |
| `invalid-argument-type` | 0 | 0 | 1 |
| **Total** | **3** | **0** | **8** |


**[Full report with detailed diff](https://c8bc46ad.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://c8bc46ad.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @Gankra on 2026-01-08 02:01_

https://github.com/DataDog/dd-trace-py/blob/fd559433b6cc12ad3205de1a16915a7ea124b277/ddtrace/sourcecode/setuptools_auto.py#L5-L9

> DEV: We have to import setuptools first who will make sure distutils is available for us.

Hey what the fuck? What on earth is happening here that we *ever* actually handled this properly.

---

_Comment by @charliermarsh on 2026-01-08 02:02_

Hahaha

---

_Label `ecosystem-analyzer` removed by @Gankra on 2026-01-08 03:31_

---

_Label `ecosystem-analyzer` added by @Gankra on 2026-01-08 03:31_

---

_Review requested from @BurntSushi by @BurntSushi on 2026-01-09 15:25_

---

_Label `ecosystem-analyzer` removed by @Gankra on 2026-01-09 15:28_

---

_Label `ecosystem-analyzer` added by @Gankra on 2026-01-09 15:28_

---

_Comment by @Gankra on 2026-01-09 15:30_

I've dropped the reordering of `-stubs` packages because I couldn't figure out how to make it coherent (notably: setuptools-types introduces `distutils-stubs`, but `distutils-stubs.core` doesn't exist, so hoisting it over the stdlib is wrong, but trying to treat typeshed as a `-stubs` to fix that ordering means we break "first party packages can shadow typeshed".

---

_Label `ecosystem-analyzer` removed by @Gankra on 2026-01-09 17:45_

---

_Label `ecosystem-analyzer` added by @Gankra on 2026-01-09 17:45_

---

_Comment by @Gankra on 2026-01-09 17:55_

I've tentatively introduced a rule that `foo/__init__.py` should shadow `foo.py` *regardless of search-path order* in attempt to handle a situation that occurs in pytest's own tests:

* [pytest defines `src/py.py` as a polyfill for `py` (aka pylib) not being installed](https://github.com/pytest-dev/pytest/blob/main/src/py.py), with a comment claiming `py/__init__.py` will shadow it if it exists.
* mypy-primer ensures `py` is installed, which provides: `py/__init__.py`, `py__init__.pyi`, and `py/path.pyi`. However this is almost assuredly on a lower-priority search-path since pytest is presumably first-party code here.

`py.py` is an evil polyfill that we cannot understand the submodules of, so if we ever resolve the module `py` as `py.py` we will fail to resolve `py.path`. `py/__init__.pyi` and `py/path.pyi` are totally comprehensible and we can easily resolve `py.path` as either an import or an attribute access.

Previously this worked because when resolving `import py.path` the search algorithm would try to find a *package* `py` and actually ignore `py.py` in the top-level, concluding its search-path has nothing as if it was a namespace package.

---

_Comment by @Gankra on 2026-01-09 18:00_

I need to confirm if this reflects runtime behaviour.

Also this is a bit sad because it now means finding `functools.pyi` in typeshed should *not* be considered the end of the story, and we need to keep checking search-paths for `functools/__init__.py(i)`. I've adjusted the invalidation test to use `functools/__init__.pyi` in typeshed as that still short-circuits and is reasonably common.

(unshadowable builtin modules also defacto short-circuit by filtering out all non-stdlib searchpaths)

---
