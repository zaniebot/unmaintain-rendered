```yaml
number: 20523
title: "[ty] More precise type inference for dictionary literals"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/bidirectional-inference
created_at: 2025-09-22T21:37:30Z
updated_at: 2025-09-24T22:13:37Z
url: https://github.com/astral-sh/ruff/pull/20523
synced_at: 2026-01-10T17:40:28Z
```

# [ty] More precise type inference for dictionary literals

---

_Pull request opened by @ibraheemdev on 2025-09-22 21:37_

## Summary

Extends https://github.com/astral-sh/ruff/pull/20360 to dictionary literals. This also improves our `TypeDict` support by passing through nested type context.

---

_Review requested from @carljm by @ibraheemdev on 2025-09-22 21:37_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-09-22 21:37_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-09-22 21:37_

---

_Review requested from @dcreager by @ibraheemdev on 2025-09-22 21:37_

---

_Label `ty` added by @ibraheemdev on 2025-09-22 21:37_

---

_Comment by @github-actions[bot] on 2025-09-22 21:39_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-24 20:21:35.472748409 +0000
+++ new-output.txt	2025-09-24 20:21:38.636768402 +0000
@@ -39,7 +39,7 @@
 aliases_implicit.py:107:9: error[invalid-type-form] Variable of type `list[Unknown | <class 'int'> | <class 'str'>]` is not allowed in a type expression
 aliases_implicit.py:108:9: error[invalid-type-form] Variable of type `tuple[tuple[<class 'int'>, <class 'str'>]]` is not allowed in a type expression
 aliases_implicit.py:109:9: error[invalid-type-form] Variable of type `list[@Todo(list comprehension element type)]` is not allowed in a type expression
-aliases_implicit.py:110:9: error[invalid-type-form] Variable of type `dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is not allowed in a type expression
+aliases_implicit.py:110:9: error[invalid-type-form] Variable of type `dict[Unknown | str, Unknown | str]` is not allowed in a type expression
 aliases_implicit.py:114:9: error[invalid-type-form] Variable of type `Literal[3]` is not allowed in a type expression
 aliases_implicit.py:115:10: error[invalid-type-form] Variable of type `Literal[True]` is not allowed in a type expression
 aliases_implicit.py:116:10: error[invalid-type-form] Variable of type `Literal[1]` is not allowed in a type expression
@@ -133,7 +133,7 @@
 classes_classvar.py:38:11: error[invalid-type-form] Type qualifier `typing.ClassVar` expected exactly 1 argument, got 2
 classes_classvar.py:39:14: error[invalid-type-form] Int literals are not allowed in this context in a type expression
 classes_classvar.py:40:14: error[unresolved-reference] Name `var` used when not defined
-classes_classvar.py:52:5: error[invalid-assignment] Object of type `dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is not assignable to `list[str]`
+classes_classvar.py:52:5: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to `list[str]`
 classes_classvar.py:55:17: error[invalid-type-form] Type qualifier `typing.ClassVar` is not allowed in type expressions (only in annotation expressions)
 classes_classvar.py:69:23: error[invalid-type-form] `ClassVar` is not allowed in function parameter annotations
 classes_classvar.py:70:12: error[invalid-type-form] `ClassVar` annotations are only allowed in class-body scopes
@@ -843,7 +843,7 @@
 tuples_type_form.py:15:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[""]]` is not assignable to `tuple[int, int]`
 tuples_type_form.py:25:1: error[invalid-assignment] Object of type `tuple[Literal[1]]` is not assignable to `tuple[()]`
 tuples_type_form.py:36:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[2], Literal[3], Literal[""]]` is not assignable to `tuple[int, ...]`
-typeddicts_operations.py:37:5: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
+typeddicts_operations.py:37:20: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_operations.py:62:1: error[unresolved-attribute] Type `MovieOptional` has no attribute `clear`
 typeddicts_readonly.py:24:4: error[invalid-assignment] Cannot assign to key "members" on TypedDict `Band`: key is marked read-only
 typeddicts_readonly.py:50:4: error[invalid-assignment] Cannot assign to key "title" on TypedDict `Movie1`: key is marked read-only
@@ -851,13 +851,14 @@
 typeddicts_readonly.py:60:4: error[invalid-assignment] Cannot assign to key "title" on TypedDict `Movie2`: key is marked read-only
 typeddicts_readonly.py:61:4: error[invalid-assignment] Cannot assign to key "year" on TypedDict `Movie2`: key is marked read-only
 typeddicts_readonly_inheritance.py:36:4: error[invalid-assignment] Cannot assign to key "name" on TypedDict `Album2`: key is marked read-only
-typeddicts_readonly_inheritance.py:65:1: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `RequiredName` constructor
+typeddicts_readonly_inheritance.py:65:19: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `RequiredName` constructor
 typeddicts_type_consistency.py:69:21: error[invalid-key] Invalid key access on TypedDict `A3`: Unknown key "y"
-typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `@Todo(dict literal value type) | None` is not assignable to `str`
+typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
+typeddicts_type_consistency.py:126:56: error[invalid-argument-type] Invalid argument to key "inner_key" with declared type `str` on TypedDict `Inner1`: value of type `Literal[1]`
 typeddicts_usage.py:23:7: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "director"
 typeddicts_usage.py:24:17: error[invalid-assignment] Invalid assignment to key "year" with declared type `int` on TypedDict `Movie`: value of type `Literal["1982"]`
-typeddicts_usage.py:28:1: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
+typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 860 diagnostics
+Found 861 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-09-22 21:50_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidirectional-inference?runnerMode=WallTime)

### Merging #20523 will **degrade performances by 50.92%**

<sub>Comparing <code>ibraheem/bidirectional-inference</code> (8db7d23) with <code>main</code> (c361e2f)</sub>



### Summary

`❌ 4` regressions  
`✅ 4` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidirectional-inference?runnerMode=WallTime)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` medium[colour-science] `` | 9.6 s | 10.4 s | -7.77% |
| ❌ | `` medium[pandas] `` | 30.8 s | 32.6 s | -5.61% |
| ❌ | `` small[freqtrade] `` | 4.1 s | 8.3 s | -50.92% |
| ❌ | `` small[pydantic] `` | 2.3 s | 2.4 s | -4.35% |


---

_Comment by @codspeed-hq[bot] on 2025-09-22 21:50_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidirectional-inference?runnerMode=Instrumentation)

### Merging #20523 will **not alter performance**

<sub>Comparing <code>ibraheem/bidirectional-inference</code> (8db7d23) with <code>main</code> (c361e2f)</sub>



### Summary

`✅ 13` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidirectional-inference?runnerMode=Instrumentation&sectionId=benchmark-comparison-section-baseline-result-skipped).


---

_Comment by @ibraheemdev on 2025-09-22 22:30_

It looks like mypy-primer is timing out due to some more issue related to union normalization. I opened https://github.com/astral-sh/ty/issues/1237 to track this issue more generally.

---

_Comment by @carljm on 2025-09-23 00:38_

It looks like the conformance suite regressions are because we don't understand the function form for creating a `TypedDict` yet. It may be that in order to prevent adding similar false positives in user code, we either need to add basic support for understanding the function form of `TypedDict` (shouldn't be too hard, I don't think?) or at least recognize it and insert a `@Todo` type. (Will be interesting to see if this also shows up as primer false positives, once we get a primer run.)

---

_Comment by @carljm on 2025-09-23 00:40_

I think we should switch primer to run in release mode and see how much that helps here. IIRC running it in debug mode originally wasn't really due to needing debug info in the run, it was because at the time it was fast enough in debug mode that saving the build time made it faster overall. I doubt that's the case today. We have enough real perf issues to sort out, we don't need to be also blocked by issues that only show up in debug builds.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:5329 on 2025-09-23 01:11_

This looks like a copy-paste error?

We should add a test that would catch this.

---

_@carljm reviewed on 2025-09-23 01:18_

This looks great! Just need to get a mypy run so we can see what surfaces there.

---

_@ibraheemdev reviewed on 2025-09-23 19:16_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:5329 on 2025-09-23 19:16_

I don't actually know if this case is ever hit, because the constraint solver only fails if an inferred type fails to meet the upper bound of a type variable.

---

_Comment by @AlexWaygood on 2025-09-23 19:36_

you probably want to rebase on top of `main` now https://github.com/astral-sh/ruff/pull/20540 has landed

Edit: argh, and I see you filed https://github.com/astral-sh/ruff/pull/20537 before I made that PR, and I didn't see it 🤦

---

_Comment by @github-actions[bot] on 2025-09-23 20:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
+ mypy_primer/main.py:67:87: error[invalid-argument-type] Argument to function `setup_mypy` is incorrect: Expected `str | None`, found `Unknown | str | None | int | RustBuildMode | Path`
+ mypy_primer/main.py:67:87: error[invalid-argument-type] Argument to function `setup_mypy` is incorrect: Expected `int | None`, found `Unknown | str | None | int | RustBuildMode | Path`
+ mypy_primer/main.py:67:87: error[invalid-argument-type] Argument to function `setup_mypy` is incorrect: Expected `bool`, found `Unknown | str | None | int | RustBuildMode | Path`
+ mypy_primer/main.py:67:87: error[invalid-argument-type] Argument to function `setup_pyright` is incorrect: Expected `str | None`, found `Unknown | str | None | int | RustBuildMode | Path`
+ mypy_primer/main.py:67:87: error[invalid-argument-type] Argument to function `setup_ty` is incorrect: Expected `RustBuildMode`, found `Unknown | str | None | int | RustBuildMode | Path`
+ mypy_primer/main.py:67:87: error[invalid-argument-type] Argument to function `setup_ty` is incorrect: Expected `str | None`, found `Unknown | str | None | int | RustBuildMode | Path`
+ mypy_primer/main.py:67:87: error[invalid-argument-type] Argument to function `setup_pyrefly` is incorrect: Expected `RustBuildMode`, found `Unknown | str | None | int | RustBuildMode | Path`
+ mypy_primer/main.py:67:87: error[invalid-argument-type] Argument to function `setup_pyrefly` is incorrect: Expected `str | None`, found `Unknown | str | None | int | RustBuildMode | Path`
+ mypy_primer/main.py:67:87: error[invalid-argument-type] Argument to function `setup_pyrefly` is incorrect: Expected `Path | None`, found `Unknown | str | None | int | RustBuildMode | Path`
- Found 3 diagnostics
+ Found 12 diagnostics

attrs (https://github.com/python-attrs/attrs)
+ src/attr/_funcs.py:487:44: error[invalid-argument-type] Argument to function `get_type_hints` is incorrect: Expected `dict[str, Any] | None`, found `Unknown | None | bool`
+ src/attr/_funcs.py:487:44: error[invalid-argument-type] Argument to function `get_type_hints` is incorrect: Expected `Mapping[str, Any] | None`, found `Unknown | None | bool`
+ src/attr/_funcs.py:487:44: error[invalid-argument-type] Argument to function `get_type_hints` is incorrect: Expected `bool`, found `Unknown | None | bool`
- src/attr/_make.py:261:12: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `dict[@Todo, @Todo] | Mapping[str, object]`
+ src/attr/_make.py:261:12: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `dict[Unknown, Unknown] | Mapping[str, object]`
+ tests/test_make.py:587:13: error[no-matching-overload] No overload of function `attrs` matches arguments
- tests/test_next_gen.py:444:25: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `dict[@Todo, @Todo]`
+ tests/test_next_gen.py:444:25: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `dict[Unknown | tuple[int], Unknown | int]`
- Found 565 diagnostics
+ Found 569 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
+ aioredis/client.py:916:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Connection]`, found `Unknown | str | int | None | float`
+ aioredis/client.py:916:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | None`, found `Unknown | str | int | None | float`
- Found 16 diagnostics
+ Found 18 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
+ pyinstrument/context_manager.py:40:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | None`
- Found 42 diagnostics
+ Found 43 diagnostics

kornia (https://github.com/kornia/kornia)
- kornia/contrib/models/sam/model.py:212:35: error[unresolved-attribute] Type `SamConfig` has no attribute `num_heads`
+ kornia/contrib/models/tiny_vit.py:411:74: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Any | list[@Todo] | PatchMerging | None | bool`
+ kornia/contrib/models/tiny_vit.py:411:74: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Any | list[@Todo] | PatchMerging | None | bool`
+ kornia/contrib/models/tiny_vit.py:411:74: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float | list[int | float]`, found `Any | list[@Todo] | PatchMerging | None | bool`
+ kornia/contrib/models/tiny_vit.py:411:74: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Any | list[@Todo] | PatchMerging | None | bool`
+ kornia/contrib/models/tiny_vit.py:420:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Any | list[@Todo] | PatchMerging | None | bool`
+ kornia/contrib/models/tiny_vit.py:420:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Any | list[@Todo] | PatchMerging | None | bool`
+ kornia/contrib/models/tiny_vit.py:420:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float | list[int | float]`, found `Any | list[@Todo] | PatchMerging | None | bool`
+ kornia/contrib/models/tiny_vit.py:420:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Any | list[@Todo] | PatchMerging | None | bool`
+ kornia/feature/loftr/loftr.py:105:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["d_model"]` on object of type `str`
+ kornia/feature/loftr/loftr.py:105:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: SupportsIndex, /) -> int, (index: slice[Any, Any, Any], /) -> tuple[int, ...]]` cannot be called with key of type `Literal["d_model"]` on object of type `tuple[int, int]`
+ kornia/feature/loftr/loftr.py:105:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Any | int | list[Unknown | int] | list[Unknown | str] | str | float`
+ kornia/feature/loftr/loftr.py:105:13: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ kornia/feature/loftr/loftr.py:105:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Any | int | list[Unknown | int] | list[Unknown | str] | str | float`
+ kornia/feature/loftr/loftr.py:105:55: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["temp_bug_fix"]` on object of type `str`
+ kornia/feature/loftr/loftr.py:105:55: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: SupportsIndex, /) -> int, (index: slice[Any, Any, Any], /) -> tuple[int, ...]]` cannot be called with key of type `Literal["temp_bug_fix"]` on object of type `tuple[int, int]`
+ kornia/feature/loftr/loftr.py:105:55: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ kornia/feature/loftr/loftr.py:107:53: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any]`, found `Any | str | tuple[int, int] | int | dict[Unknown | str, Unknown | int | list[Unknown | int]] | dict[Unknown | str, Unknown | int | list[Unknown | str] | str] | dict[Unknown | str, Unknown | float | int | str]`
+ kornia/feature/loftr/loftr.py:108:47: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any]`, found `Any | str | tuple[int, int] | int | dict[Unknown | str, Unknown | int | list[Unknown | int]] | dict[Unknown | str, Unknown | int | list[Unknown | str] | str] | dict[Unknown | str, Unknown | float | int | str]`
+ kornia/feature/loftr/loftr.py:110:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any]`, found `Any | str | tuple[int, int] | int | dict[Unknown | str, Unknown | int | list[Unknown | int]] | dict[Unknown | str, Unknown | int | list[Unknown | str] | str] | dict[Unknown | str, Unknown | float | int | str]`
- Found 783 diagnostics
+ Found 801 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
+ src/bandersnatch/simple.py:222:13: warning[possibly-missing-attribute] Attribute `append` on type `Any | list[Unknown] | dict[Unknown | str, Unknown | str] | str` may be missing
+ src/bandersnatch/simple.py:290:25: warning[possibly-missing-attribute] Attribute `append` on type `Any | dict[Unknown | str, Unknown | int] | list[Unknown]` may be missing
- Found 114 diagnostics
+ Found 116 diagnostics

beartype (https://github.com/beartype/beartype)
- beartype/_check/code/_pep/pep484585/codepep484585container.py:97:77: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 586 diagnostics
+ Found 585 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/bootstrap/core.py:225:17: error[invalid-assignment] Object of type `dict[@Todo, @Todo]` is not assignable to `QueryInfo`
+ lib/spack/spack/bootstrap/core.py:225:17: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to `QueryInfo`
- lib/spack/spack/bootstrap/core.py:233:18: error[invalid-assignment] Object of type `dict[@Todo, @Todo]` is not assignable to `QueryInfo`
+ lib/spack/spack/bootstrap/core.py:233:18: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to `QueryInfo`
- lib/spack/spack/bootstrap/core.py:246:18: error[invalid-assignment] Object of type `dict[@Todo, @Todo]` is not assignable to `QueryInfo`
+ lib/spack/spack/bootstrap/core.py:246:18: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to `QueryInfo`
- lib/spack/spack/bootstrap/core.py:267:9: error[invalid-assignment] Object of type `dict[@Todo, @Todo]` is not assignable to `QueryInfo`
+ lib/spack/spack/bootstrap/core.py:267:9: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to `QueryInfo`
- lib/spack/spack/bootstrap/core.py:307:9: error[invalid-assignment] Object of type `dict[@Todo, @Todo]` is not assignable to `QueryInfo`
+ lib/spack/spack/bootstrap/core.py:307:9: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to `QueryInfo`
+ lib/spack/spack/ci/gitlab.py:408:61: error[invalid-argument-type] Argument to function `ensure_expected_target_path` is incorrect: Expected `str`, found `Unknown | str | bytes | (Unknown & ~AlwaysFalsy)`
- lib/spack/spack/operating_systems/windows_os.py:52:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/solver/requirements.py:207:52: error[invalid-argument-type] Argument to function `parse_spec_from_yaml_string` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | (list[Unknown | (Unknown & str)] & ~AlwaysFalsy)`
+ lib/spack/spack/solver/requirements.py:223:25: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `Unknown | list[Unknown | (Unknown & str)] | None`
- lib/spack/spack/test/ci.py:228:60: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Message[str, str]`, found `dict[@Todo, @Todo]`
+ lib/spack/spack/test/ci.py:228:60: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Message[str, str]`, found `dict[Unknown, Unknown]`
- lib/spack/spack/util/spack_json.py:26:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/util/spack_json.py:27:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/environment.py:1114:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MutableMapping[Unknown, Unknown]`, found `Mapping[str, Any] | dict[@Todo, @Todo]`
+ lib/spack/spack/vendor/jinja2/environment.py:1114:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MutableMapping[Unknown, Unknown]`, found `Mapping[str, Any] | dict[Unknown, Unknown]`
+ share/spack/qa/flake8_formatter.py:62:16: warning[possibly-missing-attribute] Attribute `search` on type `Unknown | str` may be missing
- Found 7528 diagnostics
+ Found 7529 diagnostics

isort (https://github.com/pycqa/isort)
+ isort/parse.py:425:29: error[invalid-assignment] Object of type `Unknown | dict[Unknown, Unknown]` is not assignable to `list[Any] | None`
+ isort/parse.py:432:33: error[invalid-assignment] Object of type `Unknown | dict[Unknown, Unknown]` is not assignable to `list[Any] | None`
+ isort/parse.py:436:33: error[invalid-assignment] Object of type `Unknown | dict[Unknown, Unknown]` is not assignable to `list[Any] | None`
+ isort/parse.py:486:21: error[invalid-assignment] Object of type `Unknown | dict[Unknown, Unknown]` is not assignable to `list[Any] | None`
- Found 31 diagnostics
+ Found 35 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/serving.py:223:12: warning[possibly-missing-attribute] Attribute `strip` on type `Unknown | tuple[int, int] | str | TextIO | bool` may be missing
+ src/werkzeug/serving.py:225:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IO[bytes]`, found `Unknown | tuple[int, int] | str | TextIO | bool`
+ src/werkzeug/wsgi.py:66:42: error[invalid-argument-type] Argument to function `get_current_url` is incorrect: Expected `bytes | None`, found `Unknown | str`
- Found 364 diagnostics
+ Found 367 diagnostics

pylint (https://github.com/pycqa/pylint)
+ pylint/checkers/unicode.py:414:17: error[invalid-argument-type] Argument to function `_map_positions_to_result` is incorrect: Expected `dict[bytes | str, _BadChar]`, found `dict[bytes, _BadChar]`
- Found 180 diagnostics
+ Found 181 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/fixtures.py:1658:24: error[invalid-return-type] Return type does not match returned value: expected `Scope`, found `Scope | (Unknown & ~None & ~((...) -> object) & ~str) | (((str, Config, /) -> Unknown) & ~((...) -> object) & ~str) | (Unknown & ~str)`
+ src/_pytest/terminal.py:1550:9: error[no-matching-overload] No overload of bound method `setdefault` matches arguments
- testing/test_assertrewrite.py:62:17: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ testing/test_monkeypatch.py:172:25: error[invalid-argument-type] Argument to bound method `setitem` is incorrect: Expected `Mapping[Literal["y"], Literal[1700]]`, found `dict[str, object] & ~AlwaysTruthy`
+ testing/test_monkeypatch.py:175:25: error[invalid-argument-type] Argument to bound method `setitem` is incorrect: Expected `Mapping[Literal["x"], Literal[1500]]`, found `dict[str, object] & ~AlwaysTruthy`
+ testing/test_pathlib.py:875:16: warning[possibly-missing-attribute] Attribute `tests` on type `Unknown | ModuleType` may be missing
+ testing/test_pathlib.py:876:16: warning[possibly-missing-attribute] Attribute `foo` on type `Unknown | ModuleType` may be missing
- Found 480 diagnostics
+ Found 485 diagnostics

aiortc (https://github.com/aiortc/aiortc)
+ src/aiortc/rtcpeerconnection.py:108:17: error[invalid-assignment] Method `__setitem__` of type `bound method dict[int, RTCRtpCodecParameters].__setitem__(key: int, value: RTCRtpCodecParameters, /) -> None` cannot be called with a key of type `int | None` and a value of type `RTCRtpCodecParameters` on object of type `dict[int, RTCRtpCodecParameters]`
+ src/aiortc/rtcpeerconnection.py:1236:17: error[invalid-assignment] Method `__setitem__` of type `bound method dict[int, RTCRtpDecodingParameters].__setitem__(key: int, value: RTCRtpDecodingParameters, /) -> None` cannot be called with a key of type `int | None` and a value of type `RTCRtpDecodingParameters` on object of type `dict[int, RTCRtpDecodingParameters]`
- Found 92 diagnostics
+ Found 94 diagnostics

paasta (https://github.com/yelp/paasta)
- paasta_tools/adhoc_tools.py:111:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `LongRunningServiceConfigDict`, found `dict[@Todo, @Todo]`
+ paasta_tools/adhoc_tools.py:111:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `LongRunningServiceConfigDict`, found `dict[Unknown, Unknown]`
- paasta_tools/cleanup_expired_autoscaling_overrides.py:136:9: error[invalid-assignment] Object of type `dict[@Todo, @Todo]` is not assignable to attribute `data` on type `Unknown | None`
+ paasta_tools/cleanup_expired_autoscaling_overrides.py:136:9: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `data` on type `Unknown | None`
- paasta_tools/cli/cmds/local_run.py:762:9: error[invalid-assignment] Object of type `dict[@Todo, @Todo]` is not assignable to `AWSSessionCreds`
+ paasta_tools/cli/cmds/local_run.py:762:9: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown]` is not assignable to `AWSSessionCreds`
- paasta_tools/cli/cmds/local_run.py:816:5: error[invalid-assignment] Object of type `dict[@Todo, @Todo]` is not assignable to `AWSSessionCreds`
+ paasta_tools/cli/cmds/local_run.py:816:5: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown]` is not assignable to `AWSSessionCreds`
- paasta_tools/cli/cmds/local_run.py:1331:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SystemPaastaConfigDict`, found `dict[@Todo, @Todo]`
+ paasta_tools/cli/cmds/local_run.py:1331:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SystemPaastaConfigDict`, found `dict[Unknown | str, Unknown | list[Unknown]]`
- paasta_tools/cli/cmds/spark_run.py:1227:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SystemPaastaConfigDict`, found `dict[@Todo, @Todo]`
+ paasta_tools/cli/cmds/spark_run.py:1227:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SystemPaastaConfigDict`, found `dict[Unknown | str, Unknown | list[Unknown]]`
+ paasta_tools/contrib/paasta_update_soa_memcpu.py:427:9: warning[possibly-missing-attribute] Attribute `append` on type `Unknown | dict[Unknown | str, Unknown] | dict[Unknown | str, Unknown | str] | list[Unknown | str]` may be missing
- paasta_tools/frameworks/native_service_config.py:176:9: error[invalid-assignment] Object of type `dict[@Todo, @Todo]` is not assignable to `TaskInfo`
+ paasta_tools/frameworks/native_service_config.py:176:9: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown | str] | dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown | list[@Todo]] | list[@Todo]] | dict[Unknown | str, Unknown | str | list[Unknown | dict[Unknown | str, Unknown | str | bool]]] | list[Unknown | dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown]]]]` is not assignable to `TaskInfo`
- paasta_tools/instance/hpa_metrics_parser.py:26:9: error[invalid-assignment] Object of type `dict[@Todo, @Todo]` is not assignable to `HPAMetricsDict`
+ paasta_tools/instance/hpa_metrics_parser.py:26:9: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to `HPAMetricsDict`
- paasta_tools/instance/hpa_metrics_parser.py:43:9: error[invalid-assignment] Object of type `dict[@Todo, @Todo]` is not assignable to `HPAMetricsDict`
+ paasta_tools/instance/hpa_metrics_parser.py:43:9: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to `HPAMetricsDict`
+ paasta_tools/instance/kubernetes.py:352:13: warning[possibly-missing-attribute] Attribute `append` on type `Unknown | str | int | list[Unknown]` may be missing
+ paasta_tools/instance/kubernetes.py:364:13: warning[possibly-missing-attribute] Attribute `append` on type `Unknown | str | int | list[Unknown]` may be missing
- paasta_tools/instance/kubernetes.py:830:12: error[invalid-return-type] Return type does not match returned value: expected `KubernetesVersionDict`, found `dict[@Todo, @Todo]`
+ paasta_tools/instance/kubernetes.py:830:12: error[invalid-return-type] Return type does not match returned value: expected `KubernetesVersionDict`, found `dict[Unknown | str, Unknown | str | int | tuple[Unknown] | list[Unknown]]`
- paasta_tools/instance/kubernetes.py:1114:12: error[invalid-return-type] Return type does not match returned value: expected `KubernetesVersionDict`, found `dict[@Todo, @Todo]`
+ paasta_tools/instance/kubernetes.py:1114:12: error[invalid-return-type] Return type does not match returned value: expected `KubernetesVersionDict`, found `dict[Unknown | str, Unknown | str | int | list[@Todo]]`
+ paasta_tools/kubernetes_tools.py:813:9: error[no-matching-overload] No overload of bound method `update` matches arguments
+ paasta_tools/kubernetes_tools.py:3177:17: warning[possibly-missing-attribute] Attribute `extend` on type `Unknown | list[Unknown] | str` may be missing
- paasta_tools/metrics/metastatus_lib.py:637:12: error[invalid-return-type] Return type does not match returned value: expected `ResourceUtilizationDict`, found `dict[@Todo, @Todo]`
+ paasta_tools/metrics/metastatus_lib.py:637:12: error[invalid-return-type] Return type does not match returned value: expected `ResourceUtilizationDict`, found `dict[Unknown | str, Unknown | ResourceInfo | int]`
- paasta_tools/metrics/metastatus_lib.py:714:12: error[invalid-return-type] Return type does not match returned value: expected `ResourceUtilizationDict`, found `dict[@Todo, @Todo]`
+ paasta_tools/metrics/metastatus_lib.py:714:12: error[invalid-return-type] Return type does not match returned value: expected `ResourceUtilizationDict`, found `dict[Unknown | str, Unknown | ResourceInfo | int]`
- paasta_tools/metrics/metrics_lib.py:98:9: error[invalid-assignment] Method `__setitem__` of type `(bound method dict[@Todo, @Todo].__setitem__(key: @Todo, value: @Todo, /) -> None) | (bound method dict[str, @Todo].__setitem__(key: str, value: @Todo, /) -> None)` cannot be called with a key of type `str | None` and a value of type `type[BaseMetrics]` on object of type `dict[@Todo, @Todo] | dict[str, @Todo]`
+ paasta_tools/metrics/metrics_lib.py:98:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, @Todo].__setitem__(key: str, value: @Todo, /) -> None` cannot be called with a key of type `str | None` and a value of type `type[BaseMetrics]` on object of type `dict[str, @Todo]`
+ paasta_tools/paastaapi/api_client.py:713:21: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | dict[Unknown, Unknown] | list[Unknown]` may be missing
+ paasta_tools/paastaapi/api_client.py:717:21: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | dict[Unknown, Unknown] | list[Unknown]` may be missing
+ paasta_tools/paastaapi/api_client.py:720:21: warning[possibly-missing-attribute] Attribute `append` on type `Unknown | None | dict[Unknown, Unknown] | list[Unknown]` may be missing
+ paasta_tools/paastaapi/api_client.py:722:21: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | dict[Unknown, Unknown] | list[Unknown]` may be missing
+ paasta_tools/paastaapi/api_client.py:725:21: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | dict[Unknown, Unknown] | list[Unknown]` may be missing
- paasta_tools/secret_providers/vault.py:202:21: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `CryptoKey`, found `dict[@Todo, @Todo]`
+ paasta_tools/secret_providers/vault.py:202:21: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `CryptoKey`, found `dict[Unknown | str, Unknown | str]`
+ paasta_tools/setup_kubernetes_cr.py:307:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["labels"]` on object of type `str`
+ paasta_tools/setup_kubernetes_cr.py:307:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | str] | dict[Unknown, Unknown]` may be missing
+ paasta_tools/setup_kubernetes_cr.py:311:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["annotations"]` on object of type `str`
+ paasta_tools/setup_kubernetes_cr.py:311:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | str] | dict[Unknown, Unknown]` may be missing
+ paasta_tools/setup_kubernetes_cr.py:314:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["labels"]` on object of type `str`
+ paasta_tools/setup_kubernetes_cr.py:314:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | str] | dict[Unknown, Unknown]` may be missing
+ paasta_tools/setup_kubernetes_cr.py:317:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["annotations"]` on object of type `str`
+ paasta_tools/setup_kubernetes_cr.py:317:5: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | str] | dict[Unknown, Unknown]` may be missing
+ paasta_tools/setup_kubernetes_cr.py:318:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["annotations"]` on object of type `str`
+ paasta_tools/setup_kubernetes_cr.py:318:5: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | str] | dict[Unknown, Unknown]` may be missing
+ paasta_tools/setup_kubernetes_cr.py:319:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["labels"]` on object of type `str`
+ paasta_tools/setup_kubernetes_cr.py:319:5: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | str] | dict[Unknown, Unknown]` may be missing
+ paasta_tools/setup_kubernetes_cr.py:320:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["labels"]` on object of type `str`
+ paasta_tools/setup_kubernetes_cr.py:320:5: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | str] | dict[Unknown, Unknown]` may be missing
+ paasta_tools/setup_kubernetes_cr.py:321:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["labels"]` on object of type `str`
+ paasta_tools/setup_kubernetes_cr.py:321:5: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | str] | dict[Unknown, Unknown]` may be missing
+ paasta_tools/setup_kubernetes_job.py:313:29: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, HpaOverride].__setitem__(key: str, value: HpaOverride, /) -> None` cannot be called with a key of type `Unknown` and a value of type `dict[Unknown | str, Unknown | (Any & ~AlwaysFalsy)]` on object of type `dict[str, HpaOverride]`
- paasta_tools/setup_prometheus_adapter_config.py:342:12: error[invalid-return-type] Return type does not match returned value: expected `PrometheusAdapterRule`, found `dict[@Todo, @Todo]`
+ paasta_tools/setup_prometheus_adapter_config.py:342:12: error[invalid-return-type] Return type does not match returned value: expected `PrometheusAdapterRule`, found `dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str] | str]`
- paasta_tools/setup_prometheus_adapter_config.py:445:12: error[invalid-return-type] Return type does not match returned value: expected `PrometheusAdapterRule`, found `dict[@Todo, @Todo]`
+ paasta_tools/setup_prometheus_adapter_config.py:445:12: error[invalid-return-type] Return type does not match returned value: expected `PrometheusAdapterRule`, found `dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str] | str]`
- paasta_tools/setup_prometheus_adapter_config.py:518:12: error[invalid-return-type] Return type does not match returned value: expected `PrometheusAdapterRule`, found `dict[@Todo, @Todo]`
+ paasta_tools/setup_prometheus_adapter_config.py:518:12: error[invalid-return-type] Return type does not match returned value: expected `PrometheusAdapterRule`, found `dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str] | str]`
- paasta_tools/setup_prometheus_adapter_config.py:614:12: error[invalid-return-type] Return type does not match returned value: expected `PrometheusAdapterRule`, found `dict[@Todo, @Todo]`
+ paasta_tools/setup_prometheus_adapter_config.py:614:12: error[invalid-return-type] Return type does not match returned value: expected `PrometheusAdapterRule`, found `dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str] | str]`
- paasta_tools/setup_prometheus_adapter_config.py:715:12: error[invalid-return-type] Return type does not match returned value: expected `PrometheusAdapterRule`, found `dict[@Todo, @Todo]`
+ paasta_tools/setup_prometheus_adapter_config.py:715:12: error[invalid-return-type] Return type does not match returned value: expected `PrometheusAdapterRule`, found `dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str] | str]`
- paasta_tools/setup_prometheus_adapter_config.py:766:12: error[invalid-return-type] Return type does not match returned value: expected `PrometheusAdapterRule`, found `dict[@Todo, @Todo]`
+ paasta_tools/setup_prometheus_adapter_config.py:766:12: error[invalid-return-type] Return type does not match returned value: expected `PrometheusAdapterRule`, found `dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str] | str]`
- paasta_tools/setup_prometheus_adapter_config.py:864:12: error[invalid-return-type] Return type does not match returned value: expected `PrometheusAdapterConfig`, found `dict[@Todo, @Todo]`
+ paasta_tools/setup_prometheus_adapter_config.py:864:12: error[invalid-return-type] Return type does not match returned value: expected `PrometheusAdapterConfig`, found `dict[Unknown | str, Unknown | list[PrometheusAdapterRule]]`
+ paasta_tools/tron/client.py:45:37: error[invalid-argument-type] Argument to function `get` is incorrect: Expected `str | bytes`, found `Unknown | str | dict[Unknown | str, Unknown | str]`
+ paasta_tools/tron/client.py:45:37: error[invalid-argument-type] Argument to function `get` is incorrect: Expected `Mapping[str, str | bytes | None] | None`, found `Unknown | str | dict[Unknown | str, Unknown | str]`
+ paasta_tools/tron/client.py:45:37: error[invalid-argument-type] Argument to function `get` is incorrect: Expected `bool`, found `Unknown | str | dict[Unknown | str, Unknown | str]`
+ paasta_tools/tron/client.py:45:37: error[invalid-argument-type] Argument to function `get` is incorrect: Expected `bool | None`, found `Unknown | str | dict[Unknown | str, Unknown | str]`
+ paasta_tools/tron/client.py:50:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | str]` may be missing
+ paasta_tools/tron/client.py:51:38: error[invalid-argument-type] Argument to function `post` is incorrect: Expected `str | bytes`, found `Unknown | str | dict[Unknown | str, Unknown | str]`
+ paasta_tools/tron/client.py:51:38: error[invalid-argument-type] Argument to function `post` is incorrect: Expected `Mapping[str, str | bytes | None] | None`, found `Unknown | str | dict[Unknown | str, Unknown | str]`
+ paasta_tools/tron/client.py:51:38: error[invalid-argument-type] Argument to function `post` is incorrect: Expected `bool`, found `Unknown | str | dict[Unknown | str, Unknown | str]`
+ paasta_tools/tron/client.py:51:38: error[invalid-argument-type] Argument to function `post` is incorrect: Expected `bool | None`, found `Unknown | str | dict[Unknown | str, Unknown | str]`
+ paasta_tools/tron_tools.py:547:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, FieldSelectorConfig]`, found `dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]`
+ paasta_tools/tron_tools.py:1096:17: error[invalid-argument-type] Argument to function `build_spark_command` is incorrect: Expected `str`, found `Unknown | list[TronSecretVolume] | bool | str | None`
- paasta_tools/utils.py:594:9: error[invalid-assignment] Object of type `list[Unknown | dict[@Todo, @Todo]]` is not assignable to `list[DockerParameter]`
+ paasta_tools/utils.py:594:9: error[invalid-assignment] Object of type `list[Unknown | dict[Unknown | str, Unknown | str]]` is not assignable to `list[DockerParameter]`
- paasta_tools/utils.py:601:17: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `DockerParameter`, found `dict[@Todo, @Todo]`
+ paasta_tools/utils.py:601:17: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `DockerParameter`, found `dict[Unknown | str, Unknown | str]`
- paasta_tools/utils.py:2112:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SystemPaastaConfigDict`, found `dict[@Todo, @Todo]`
+ paasta_tools/utils.py:2112:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SystemPaastaConfigDict`, found `dict[Unknown, Unknown]`
- paasta_tools/utils.py:2120:5: error[invalid-assignment] Object of type `dict[@Todo, @Todo]` is not assignable to `SystemPaastaConfigDict`
+ paasta_tools/utils.py:2120:5: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to `SystemPaastaConfigDict`
- paasta_tools/utils.py:3581:9: error[invalid-assignment] Object of type `dict[@Todo, @Todo]` is not assignable to `BranchDictV2`
+ paasta_tools/utils.py:3581:9: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown]` is not assignable to `BranchDictV2`
- Found 875 diagnostics
+ Found 913 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ tests/test_downloadermiddleware_httpcompression.py:405:49: error[invalid-argument-type] Argument to bound method `from_args` is incorrect: Expected `Mapping[bytes, bytes] | None`, found `dict[Unknown | str, Unknown | str]`
- tests/test_downloadermiddleware_httpproxy.py:27:9: error[invalid-assignment] Object of type `dict[@Todo, @Todo]` is not assignable to attribute `environ` of type `_Environ[str]`
+ tests/test_downloadermiddleware_httpproxy.py:27:9: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str]` is not assignable to attribute `environ` of type `_Environ[str]`
+ tests/test_downloadermiddleware_offsite.py:90:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | dict[Unknown, Unknown]`
+ tests/test_downloadermiddleware_offsite.py:90:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes | str | None`, found `Unknown | dict[Unknown, Unknown]`
+ tests/test_downloadermiddleware_offsite.py:90:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | dict[Unknown, Unknown]`
+ tests/test_downloadermiddleware_offsite.py:90:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Unknown | dict[Unknown, Unknown]`
+ tests/test_downloadermiddleware_offsite.py:90:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | dict[Unknown, Unknown]`
+ tests/test_downloadermiddleware_offsite.py:90:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((Unknown, /) -> Any) | None`, found `Unknown | dict[Unknown, Unknown]`
+ tests/test_downloadermiddleware_offsite.py:90:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str] | None`, found `Unknown | dict[Unknown, Unknown]`
+ tests/test_downloaderslotssettings.py:99:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | int`
- tests/test_feedexport.py:238:77: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `dict[@Todo, @Todo]`
+ tests/test_feedexport.py:238:77: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `dict[Unknown, Unknown]`
+ tests/test_feedexport.py:1780:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> bytes]` cannot be called with key of type `Literal["format"]` on object of type `bytes`
+ tests/test_http2_client_protocol.py:114:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | dict[Unknown, Unknown] | str` may be missing
+ tests/test_responsetypes.py:119:46: error[invalid-argument-type] Argument to bound method `from_args` is incorrect: Expected `Mapping[bytes, bytes] | None`, found `Unknown | str | Headers`
+ tests/test_responsetypes.py:119:46: error[invalid-argument-type] Argument to bound method `from_args` is incorrect: Expected `str | None`, found `Unknown | str | Headers`
+ tests/test_responsetypes.py:119:46: error[invalid-argument-type] Argument to bound method `from_args` is incorrect: Expected `str | None`, found `Unknown | str | Headers`
+ tests/test_responsetypes.py:119:46: error[invalid-argument-type] Argument to bound method `from_args` is incorrect: Expected `bytes | None`, found `Unknown | str | Headers`
- tests/test_utils_misc/__init__.py:39:25: error[invalid-argument-type] Argument to function `load_object` is incorrect: Expected `str | ((...) -> Any)`, found `dict[@Todo, @Todo]`
+ tests/test_utils_misc/__init__.py:39:25: error[invalid-argument-type] Argument to function `load_object` is incorrect: Expected `str | ((...) -> Any)`, found `dict[Unknown, Unknown]`
- Found 1044 diagnostics
+ Found 1059 diagnostics

twine (https://github.com/pypa/twine)
- twine/package.py:264:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 12 diagnostics
+ Found 11 diagnostics

dulwich (https://github.com/dulwich/dulwich)
+ dulwich/porcelain.py:1421:19: error[invalid-argument-type] Argument is incorrect: Expected `Tree`, found `Tree | Blob | Commit | Tag`
+ dulwich/porcelain.py:1421:19: error[invalid-argument-type] Argument is incorrect: Expected `Blob`, found `Tree | Blob | Commit | Tag`
+ dulwich/porcelain.py:1421:19: error[invalid-argument-type] Argument is incorrect: Expected `Commit`, found `Tree | Blob | Commit | Tag`
+ dulwich/porcelain.py:1421:19: error[invalid-argument-type] Argument is incorrect: Expected `Tag`, found `Tree | Blob | Commit | Tag`
- dulwich/tests/utils.py:369:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 177 diagnostics
+ Found 180 diagnostics

alerta (https://github.com/alerta/alerta)
+ alerta/utils/logging.py:309:22: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int` and `Unknown | str | int`
- alerta/webhooks/prometheus.py:70:41: warning[possibly-missing-attribute] Attribute `upper` on type `@Todo | None | Literal["unknown"]` may be missing
+ alerta/webhooks/prometheus.py:70:41: warning[possibly-missing-attribute] Attribute `upper` on type `Unknown | None | Literal["unknown"]` may be missing
- Found 512 diagnostics
+ Found 513 diagnostics

starlette (https://github.com/encode/starlette)
+ starlette/middleware/wsgi.py:69:21: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | str | tuple[int, int] | BytesIO | TextIO | bool` and `Literal[","]`
+ starlette/testclient.py:365:49: warning[possibly-missing-attribute] Attribute `read` on type `Unknown | int | list[Unknown] | BytesIO` may be missing
- Found 144 diagnostics
+ Found 146 diagnostics

rich (https://github.com/Textualize/rich)
- rich/pretty.py:1006:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_console.py:173:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int]`, found `(Unknown & ~<class 'ValueError'>) | tuple[int, int] | (type[ValueError] & ~<class 'ValueError'>)`

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/utilities/extend_schema.py:265:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/graphql/utilities/extend_schema.py:266:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/graphql/utilities/extend_schema.py:267:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/error/test_graphql_error.py:404:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/execution/test_defer.py:211:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | str | list[Unknown | str | int]`
+ tests/execution/test_defer.py:211:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str | int]`, found `Any | str | list[Unknown | str | int]`
+ tests/execution/test_defer.py:211:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `Any | str | list[Unknown | str | int]`
+ tests/execution/test_defer.py:212:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | str | list[Unknown | str | int]`
+ tests/execution/test_defer.py:212:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str | int]`, found `Any | str | list[Unknown | str | int]`
+ tests/execution/test_defer.py:212:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `Any | str | list[Unknown | str | int]`
+ tests/execution/test_defer.py:236:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | str | list[Unknown]`
+ tests/execution/test_defer.py:236:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | str | list[Unknown]`
+ tests/execution/test_defer.py:237:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | str | list[Unknown]`
+ tests/execution/test_defer.py:237:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | str | list[Unknown]`
+ tests/execution/test_defer.py:283:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any]`, found `Any | dict[Unknown | str, Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:283:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | dict[Unknown | str, Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:283:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str | int] | None`, found `Any | dict[Unknown | str, Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:283:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | dict[Unknown | str, Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:283:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | dict[Unknown | str, Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:284:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any]`, found `Any | dict[Unknown | str, Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:284:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | dict[Unknown | str, Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:284:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str | int] | None`, found `Any | dict[Unknown | str, Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:284:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | dict[Unknown | str, Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:284:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | dict[Unknown | str, Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:348:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | list[Unknown | PendingResult] | bool | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:348:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | list[Unknown | PendingResult] | bool | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:348:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[PendingResult] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | list[Unknown | PendingResult] | bool | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:348:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | list[Unknown | PendingResult] | bool | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:348:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | list[Unknown | PendingResult] | bool | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:349:60: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | list[Unknown | PendingResult] | bool | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:349:60: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | list[Unknown | PendingResult] | bool | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:349:60: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[PendingResult] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | list[Unknown | PendingResult] | bool | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:349:60: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | list[Unknown | PendingResult] | bool | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:349:60: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | list[Unknown | PendingResult] | bool | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:457:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:457:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[PendingResult] | None`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:457:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Unknown] | None`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:457:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[CompletedResult] | None`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:457:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:458:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:458:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[PendingResult] | None`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:458:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Unknown] | None`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:458:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[CompletedResult] | None`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:458:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:189:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Any]`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:189:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:189:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str | int] | None`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:189:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:189:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:190:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Any]`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:190:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:190:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str | int] | None`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:190:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:190:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/test_user_registry.py:578:16: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["id"]` on object of type `str`
- tests/test_user_registry.py:288:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_user_registry.py:343:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/type/test_definition.py:158:72: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `ValueNode`, found `dict[@Todo, @Todo]`
+ tests/type/test_definition.py:158:72: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `ValueNode`, found `dict[Unknown | str, Unknown | str]`
- tests/type/test_directives.py:66:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 302 diagnostics
+ Found 346 diagnostics

sockeye (https://github.com/awslabs/sockeye)
- sockeye/log.py:128:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- sockeye/log.py:130:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- sockeye/log.py:133:76: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- sockeye/log.py:134:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ sockeye/test_utils.py:224:13: error[invalid-argument-type] Argument to function `generate_json_input_file_with_tgt_prefix` is incorrect: Expected `list[str] | None`, found `None | Unknown | str | list[Unknown]`
- sockeye/test_utils.py:224:34: error[invalid-argument-type] Argument to function `generate_json_input_file_with_tgt_prefix` is incorrect: Expected `list[str]`, found `None | @Todo | list[Unknown]`
+ sockeye/test_utils.py:224:34: error[invalid-argument-type] Argument to function `generate_json_input_file_with_tgt_prefix` is incorrect: Expected `list[str]`, found `None | Unknown | str | list[Unknown]`
- Found 318 diagnostics
+ Found 315 diagnostics

ignite (https://github.com/pytorch/ignite)
- tests/ignite/handlers/test_fbresearch_logger.py:79:5: error[invalid-assignment] Object of type `dict[@Todo, @Todo]` is not assignable to attribute `output` on type `Unknown | State`
+ tests/ignite/handlers/test_fbresearch_logger.py:79:5: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | float]` is not assignable to attribute `output` on type `Unknown | State`
+ tests/ignite/handlers/test_state_param_scheduler.py:336:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | int | float | list[Unknown | int]`
+ tests/ignite/handlers/test_state_param_scheduler.py:336:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | int | float | list[Unknown | int]`
+ tests/ignite/handlers/test_state_param_scheduler.py:336:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[int]`, found `Unknown | str | int | float | list[Unknown | int]`
+ tests/ignite/handlers/test_state_param_scheduler.py:336:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | int | float | list[Unknown | int]`
+ tests/ignite/handlers/test_state_param_scheduler.py:336:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | int | float | list[Unknown | int]`
+ tests/ignite/handlers/test_state_param_scheduler.py:336:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | int | float | list[Unknown | int]`
+ tests/ignite/handlers/test_state_param_scheduler.py:342:84: error[invalid-argument-type] Argument to bound method `plot_values` is incorrect: Expected `Mapping[Unknown, Unknown]`, found `Unknown | str | int | float | list[Unknown | int]`
+ tests/ignite/metrics/test_metric.py:1214:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `_num_correct` on type `Metric | Unknown`
+ tests/ignite/metrics/test_metric.py:1215:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `_num_examples` on type `Metric | Unknown`
+ tests/ignite/metrics/test_metric.py:1216:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `_numerator` on type `Metric | Unknown`
+ tests/ignite/metrics/test_metric.py:1217:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `_denominator` on type `Metric | Unknown`
+ tests/ignite/metrics/test_metric.py:1218:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `_weight` on type `Metric | Unknown`
+ tests/ignite/metrics/test_metric.py:1219:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `_updated` on type `Metric | Unknown`
- Found 2140 diagnostics
+ Found 2153 diagnostics

porcupine (https://github.com/Akuli/porcupine)
+ porcupine/plugins/restart.py:64:15: warning[possibly-missing-attribute] Attribute `from_state` on type `@Todo | bool` may be missing
- Found 25 diagnostics
+ Found 26 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ pydantic/__init__.py:247:1: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | tuple[str | None, str]]` is not assignable to `dict[str, tuple[str, str]]`
+ pydantic/_internal/_dataclasses.py:223:12: error[no-matching-overload] No overload of function `field` matches arguments
+ pydantic/_internal/_generate_schema.py:1942:9: error[invalid-assignment] Object of type `dict[Unknown | _ParameterKind, Unknown | str]` is not assignable to `dict[_ParameterKind, Literal["positional_only", "positional_or_keyword", "keyword_only"]]`
+ pydantic/_internal/_generate_schema.py:2016:9: error[invalid-assignment] Object of type `dict[Unknown | _ParameterKind, Unknown | str]` is not assignable to `dict[_ParameterKind, Literal["positional_only", "positional_or_keyword", "var_args", "keyword_only"]]`
+ pydantic/_internal/_generate_schema.py:2441:1: error[invalid-assignment] Object of type `dict[Unknown | tuple[str, str], Unknown | ((f, schema) -> Unknown) | ((f, _) -> Unknown)]` is not assignable to `Mapping[tuple[@Todo, Literal["no-info", "with-info"]], ((...) -> Any, Unknown, /) -> Unknown]`
- Found 760 diagnostics
+ Found 765 diagnostics

mkosi (https://github.com/systemd/mkosi)
- mkosi/config.py:290:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `@Todo | None`
+ mkosi/config.py:290:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | str | None`
- mkosi/config.py:496:16: error[invalid-return-type] Return type does not match returned value: expected `Architecture`, found `@Todo | None`
+ mkosi/config.py:496:16: error[invalid-return-type] Return type does not match returned value: expected `Architecture`, found `Unknown | Architecture | None`
- mkosi/config.py:539:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `@Todo | None`
+ mkosi/config.py:539:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | str | None`
- mkosi/config.py:561:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `@Todo | None`
+ mkosi/config.py:561:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | str | None`
- mkosi/distributions/arch.py:125:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `@Todo | None`
+ mkosi/distributions/arch.py:125:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | str | None`
- mkosi/distributions/azure.py:111:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `@Todo | None`
+ mkosi/distributions/azure.py:111:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | str | None`
- mkosi/distributions/centos.py:96:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `@Todo | None`
+ mkosi/distributions/centos.py:96:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | str | None`
- mkosi/distributions/debian.py:244:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `@Todo | None`
+ mkosi/distributions/debian.py:244:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | str | None`
- mkosi/distributions/fedora.py:259:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `@Todo | None`
+ mkosi/distributions/fedora.py:259:16: error[in...*[Comment body truncated]*

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-09-23 20:11_

---

_Comment by @github-actions[bot] on 2025-09-23 20:16_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 982 | 0 | 122 |
| `unknown-argument` | 694 | 0 | 0 |
| `possibly-missing-attribute` | 195 | 2 | 25 |
| `invalid-assignment` | 119 | 0 | 43 |
| `possibly-missing-implicit-call` | 153 | 0 | 7 |
| `non-subscriptable` | 102 | 0 | 2 |
| `unsupported-operator` | 71 | 1 | 13 |
| `unused-ignore-comment` | 0 | 79 | 0 |
| `no-matching-overload` | 52 | 0 | 0 |
| `invalid-return-type` | 15 | 0 | 32 |
| `call-non-callable` | 43 | 0 | 1 |
| `index-out-of-bounds` | 15 | 0 | 0 |
| `too-many-positional-arguments` | 12 | 0 | 0 |
| `not-iterable` | 7 | 0 | 4 |
| `missing-argument` | 9 | 0 | 0 |
| `unresolved-attribute` | 2 | 2 | 2 |
| `type-assertion-failure` | 0 | 1 | 0 |
| **Total** | **2,471** | **85** | **251** |

**[Full report with detailed diff](https://ibraheem-bidirectional-infer.ecosystem-663.pages.dev/diff)**


---

_Comment by @ibraheemdev on 2025-09-23 20:24_

From a quick skim, a lot of the errors seem to be of the form:
```py
def f(a: int, b: str): ...

x = { "a": 1, "b": "2" }

# Expected `int` found `Unknown | int | str`
f(x["a"], x["b"])
# or
f(**x)
```
There were similar errors in the list/set inference PR, but it seems like a more common pattern in the ecosystem for dictionaries. pyright again accepts this, as it doesn't even attempt to infer a union and falls back to `dict[str, Unknown]`, but mypy widens to `dict[str, object]`, and also errors.

---

_Comment by @carljm on 2025-09-24 16:02_

I can see the rationale for pyright's behavior. This is a case where we can emit a lot of false positives on perfectly working untyped code, just because we don't infer a sufficiently precise type for the dict literal (which is implicitly used as a heterogeneous `TypedDict`). Ideally maybe this would be solved by a more precise internal "implicit TypedDict" type, but that gets really complex to handle with subsequent mutations.

I think for now we should probably go ahead with this as-is, but in the future in "gradual guarantee" mode we may want to emulate pyright for container literals that "look heterogeneous".

---

_@carljm approved on 2025-09-24 16:03_

---

_Comment by @carljm on 2025-09-24 16:11_

Filed https://github.com/astral-sh/ty/issues/1248 for follow-up later.

---

_Comment by @ibraheemdev on 2025-09-24 20:21_

> we either need to add basic support for understanding the function form of TypedDict (shouldn't be too hard, I don't think?) or at least recognize it and insert a `@Todo` type

I think this is actually tricky, because we don't currently have infrastructure for any other functional type forms (e.g. `NamedTuple`, or `NewType`), so we should probably wait till something like https://github.com/astral-sh/ruff/pull/20126 lands.

I added a fallback to `dict[Unknown, Unknown]` if the type annotation is a `@Todo` type for now.

---

_Comment by @carljm on 2025-09-24 20:33_

> I think this is actually tricky, because we don't currently have infrastructure for any other functional type forms

I think the workaround you put in place is fine for this PR.

I am curious what the trickiest part of functional `TypedDict` and `NamedTuple` would be. I think the hard parts of the `NewType` PR are in how to represent the "new-type" itself, which I don't think would be common infrastructure shared with `TypedDict` and `NamedTuple`? I think we'd need a fully-materialized (as opposed to "lazy based on a class definition") representation of those types; similar to `Protocol::Synthesized` vs `Protocol::FromClass`.

Nothing actionable for this PR, I'm just not convinced that landing the `NewType` PR is a pre-requisite to support for functional `TypedDict` or `NamedTuple`. (Which maybe doesn't matter either, since `NewType` may be just as high priority anyway.)

---

_@ibraheemdev reviewed on 2025-09-24 20:35_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:5326 on 2025-09-24 20:35_

We don't have access to the target binding anymore, so this changes diagnostics slightly. Before:
```
error[missing-typed-dict-key]: Missing required key 'name' in TypedDict `Movie` constructor
 --> x.py:8:5
  |
7 | def func1(variable_key: str):
8 |     movie: Movie = {variable_key: "", "year": 1900}
  |     ^^^^^
```
After:
```
error[missing-typed-dict-key]: Missing required key 'name' in TypedDict `Movie` constructor
 --> x.py:8:20
  |
7 | def func1(variable_key: str):
8 |     movie: Movie = {variable_key: "", "year": 1900}
  |                    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```
I think this makes sense, as it extends nicely to errors in nested `TypedDict` literals.

---

_@carljm reviewed on 2025-09-24 20:37_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:5326 on 2025-09-24 20:37_

I think this is fine. 

---

_Merged by @ibraheemdev on 2025-09-24 22:12_

---

_Closed by @ibraheemdev on 2025-09-24 22:12_

---

_Branch deleted on 2025-09-24 22:12_

---

_Comment by @ibraheemdev on 2025-09-24 22:13_

The performance regression on `freqtrade` comes from inferring the type of [this massive dictionary literal](https://github.com/freqtrade/freqtrade/blob/develop/freqtrade/config_schema/config_schema.py#L31), which we were previously ignoring. I'm not sure there's much that can be done there.

---
