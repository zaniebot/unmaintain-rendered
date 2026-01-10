```yaml
number: 21723
title: "[ty] Fix false positives for `class F(Generic[*Ts]): ...`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/generic-todo
created_at: 2025-12-01T11:03:02Z
updated_at: 2025-12-01T13:24:09Z
url: https://github.com/astral-sh/ruff/pull/21723
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Fix false positives for `class F(Generic[*Ts]): ...`

---

_Pull request opened by @AlexWaygood on 2025-12-01 11:03_

## Summary

I believe this gets rid of around 5 `invalid-argument-type` diagnostics on `scipy-stubs` (relates to https://github.com/astral-sh/ty/issues/1685).

## Test Plan

mdtest added


---

_Label `ty` added by @AlexWaygood on 2025-12-01 11:03_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 11:05_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-01 11:04:50.844708736 +0000
+++ new-output.txt	2025-12-01 11:04:54.383730028 +0000
@@ -458,13 +458,10 @@
 generics_defaults.py:80:53: error[invalid-type-arguments] Too many type arguments to class `Class_ParamSpec`: expected between 0 and 1, got 2
 generics_defaults.py:81:29: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[bool, bool]`?
 generics_defaults.py:81:68: error[invalid-type-arguments] Too many type arguments to class `Class_ParamSpec`: expected between 0 and 1, got 2
-generics_defaults.py:91:26: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_defaults.py:94:1: error[type-assertion-failure] Type `@Todo(specialized non-generic class)` does not match asserted type `<class 'Class_TypeVarTuple'>`
 generics_defaults.py:95:1: error[type-assertion-failure] Type `@Todo(specialized non-generic class)` does not match asserted type `Class_TypeVarTuple`
 generics_defaults.py:127:32: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T4@func1`
 generics_defaults.py:131:1: error[type-assertion-failure] Type `Any` does not match asserted type `int`
-generics_defaults.py:142:12: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
-generics_defaults.py:152:12: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_defaults.py:155:49: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int | float, bool]`?
 generics_defaults.py:156:58: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `list[bytes]`?
 generics_defaults.py:170:1: error[type-assertion-failure] Type `(Foo7[int], /) -> Foo7[int]` does not match asserted type `def meth(self, /) -> Self@meth`
@@ -600,16 +597,13 @@
 generics_typevartuple_args.py:27:77: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_args.py:31:1: error[type-assertion-failure] Type `tuple[()]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_args.py:32:1: error[type-assertion-failure] Type `tuple[int, str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_basic.py:12:14: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_basic.py:16:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_basic.py:23:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_basic.py:42:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[@Todo(PEP 646), ...]`, found `Height`
 generics_typevartuple_basic.py:65:27: error[unknown-argument] Argument `covariant` does not match any known parameter of function `__new__`
 generics_typevartuple_basic.py:66:27: error[too-many-positional-arguments] Too many positional arguments to function `__new__`: expected 2, got 4
 generics_typevartuple_basic.py:67:27: error[unknown-argument] Argument `bound` does not match any known parameter of function `__new__`
 generics_typevartuple_basic.py:75:50: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_basic.py:84:1: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_basic.py:106:14: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_callable.py:29:57: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_callable.py:33:54: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[int | float | complex, str, int]`
 generics_typevartuple_callable.py:37:34: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[str]`
@@ -617,17 +611,13 @@
 generics_typevartuple_callable.py:42:1: error[type-assertion-failure] Type `tuple[str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_callable.py:45:43: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_callable.py:49:1: error[type-assertion-failure] Type `tuple[int | float, str, int | float | complex]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_concat.py:22:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_concat.py:47:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_concat.py:52:1: error[type-assertion-failure] Type `tuple[int, bool, str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_overloads.py:16:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
-generics_typevartuple_specialization.py:16:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_specialization.py:45:23: error[invalid-type-arguments] Too many type arguments: expected 0, got 2
 generics_typevartuple_specialization.py:45:51: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
 generics_typevartuple_specialization.py:46:5: error[type-assertion-failure] Type `tuple[int, int | float, bool]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_specialization.py:51:5: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_specialization.py:52:37: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
-generics_typevartuple_specialization.py:59:14: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_specialization.py:92:28: error[invalid-type-arguments] Too many type arguments: expected 0, got 2
 generics_typevartuple_specialization.py:92:56: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
 generics_typevartuple_specialization.py:93:5: error[type-assertion-failure] Type `tuple[str, int]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
@@ -657,7 +647,6 @@
 generics_typevartuple_specialization.py:158:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_specialization.py:159:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_specialization.py:163:13: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
-generics_typevartuple_unpack.py:17:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_upper_bound.py:37:1: error[type-assertion-failure] Type `list[int]` does not match asserted type `list[Unknown | int]`
 generics_upper_bound.py:38:1: error[type-assertion-failure] Type `set[int]` does not match asserted type `set[Unknown | int]`
 generics_upper_bound.py:43:1: error[type-assertion-failure] Type `list[int] | set[int]` does not match asserted type `list[Unknown | int] | set[Unknown | int]`
@@ -1039,4 +1028,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1041 diagnostics
+Found 1030 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-01 11:06_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- src/anyio/_core/_sockets.py:385:56: error[invalid-argument-type] Argument to function `setup_raw_socket` is incorrect: Expected `tuple[str, int] | tuple[str, int, int, int]`, found `tuple[str | Unknown, int, @Todo]`
+ src/anyio/_core/_sockets.py:385:56: error[invalid-argument-type] Argument to function `setup_raw_socket` is incorrect: Expected `tuple[str, int] | tuple[str, int, int, int]`, found `tuple[str | Unknown, int, @Todo(StarredExpression)]`

spack (https://github.com/spack/spack)
- lib/spack/spack/llnl/util/link_tree.py:116:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str, str]`, found `tuple[str, @Todo]`
+ lib/spack/spack/llnl/util/link_tree.py:116:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str, str]`, found `tuple[str, @Todo(StarredExpression)]`
- lib/spack/spack/llnl/util/link_tree.py:151:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str, str]`, found `tuple[str, @Todo]`
+ lib/spack/spack/llnl/util/link_tree.py:151:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str, str]`, found `tuple[str, @Todo(StarredExpression)]`

black (https://github.com/psf/black)
- src/black/cache.py:144:59: error[invalid-assignment] Object of type `dict[str | Unknown, tuple[@Todo] | Unknown]` is not assignable to `dict[str, tuple[int | float, int, str]]`
+ src/black/cache.py:144:59: error[invalid-assignment] Object of type `dict[str | Unknown, tuple[@Todo(StarredExpression)] | Unknown]` is not assignable to `dict[str, tuple[int | float, int, str]]`

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/reports.py:302:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, int, str]`, found `tuple[@Todo, str]`
+ src/_pytest/reports.py:302:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, int, str]`, found `tuple[@Todo(StarredExpression), str]`
- src/_pytest/terminal.py:1582:23: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[int, str, int | None, str]`, found `tuple[int, @Todo]`
+ src/_pytest/terminal.py:1582:23: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[int, str, int | None, str]`, found `tuple[int, @Todo(StarredExpression)]`

poetry (https://github.com/python-poetry/poetry)
- src/poetry/utils/env/mock_env.py:34:28: error[invalid-assignment] Object of type `tuple[@Todo, Literal["final"], Literal[0]]` is not assignable to `tuple[int, int, int] | tuple[int, int, int, str, int]`
+ src/poetry/utils/env/mock_env.py:34:28: error[invalid-assignment] Object of type `tuple[@Todo(StarredExpression), Literal["final"], Literal[0]]` is not assignable to `tuple[int, int, int] | tuple[int, int, int, str, int]`

vision (https://github.com/pytorch/vision)
- torchvision/datasets/utils.py:268:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str | None, str | None]`, found `tuple[@Todo, @Todo]`
+ torchvision/datasets/utils.py:268:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str | None, str | None]`, found `tuple[@Todo, @Todo(StarredExpression)]`

apprise (https://github.com/caronc/apprise)
- tests/test_plugin_email.py:634:5: error[invalid-assignment] Object of type `tuple[@Todo, @Todo]` is not assignable to attribute `EMAIL_TEMPLATES` of type `tuple[tuple[Literal["Google Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Yandex"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Hotmail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Outlook"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Office 365"], Pattern[str], dict[Unknown | str, Unknown | int | str]], tuple[Literal["Yahoo Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Fast Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Fast Mail Extended Addresses"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Zoho Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["SendGrid"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["163.com"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Foxmail.com"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Comcast.net"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Local Mail Server"], Pattern[str], dict[Unknown | str, Unknown | str | None]], tuple[Literal["Custom"], Pattern[str], dict[Unknown | str, Unknown | None]]]`
+ tests/test_plugin_email.py:634:5: error[invalid-assignment] Object of type `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` is not assignable to attribute `EMAIL_TEMPLATES` of type `tuple[tuple[Literal["Google Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Yandex"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Hotmail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Outlook"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Office 365"], Pattern[str], dict[Unknown | str, Unknown | int | str]], tuple[Literal["Yahoo Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Fast Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Fast Mail Extended Addresses"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Zoho Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["SendGrid"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["163.com"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Foxmail.com"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Comcast.net"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Local Mail Server"], Pattern[str], dict[Unknown | str, Unknown | str | None]], tuple[Literal["Custom"], Pattern[str], dict[Unknown | str, Unknown | None]]]`

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/utils/isbn.py:112:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[str | None, str | None, str | None]`, found `tuple[str | None, @Todo]`
+ openlibrary/utils/isbn.py:112:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[str | None, str | None, str | None]`, found `tuple[str | None, @Todo(StarredExpression)]`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 41 diagnostics
+ Found 42 diagnostics

scipy-stubs (https://github.com/scipy/scipy-stubs)
- scipy-stubs/_lib/_ccallback.pyi:60:28: error[invalid-argument-type] `@Todo` is not a valid argument to `Protocol`
- scipy-stubs/_lib/_ccallback.pyi:75:46: error[invalid-argument-type] `@Todo` is not a valid argument to `Protocol`
- scipy-stubs/integrate/_ode.pyi:45:11: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
- scipy-stubs/integrate/_ode.pyi:92:45: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
- scipy-stubs/stats/_typing.pyi:53:55: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
- Found 1971 diagnostics
+ Found 1966 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/models/__init__.py:108:11: error[invalid-assignment] Object of type `tuple[Literal["Model"], @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo]` is not assignable to `tuple[@Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo]`
+ src/bokeh/models/__init__.py:108:11: error[invalid-assignment] Object of type `tuple[Literal["Model"], @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression)]` is not assignable to `tuple[@Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression)]`
- src/bokeh/models/annotations/__init__.py:44:11: error[invalid-assignment] Object of type `tuple[@Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo]` is not assignable to `tuple[@Todo, @Todo, @Todo]`
+ src/bokeh/models/annotations/__init__.py:44:11: error[invalid-assignment] Object of type `tuple[@Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression)]` is not assignable to `tuple[@Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression)]`

jax (https://github.com/google/jax)
- jax/_src/pallas/hlo_interpreter.py:399:33: error[invalid-argument-type] Argument to function `pad` is incorrect: Expected `Sequence[tuple[int, int, int]]`, found `list[tuple[@Todo, int] | Unknown]`
+ jax/_src/pallas/hlo_interpreter.py:399:33: error[invalid-argument-type] Argument to function `pad` is incorrect: Expected `Sequence[tuple[int, int, int]]`, found `list[tuple[@Todo(StarredExpression), int] | Unknown]`
- jax/experimental/mosaic/gpu/utils.py:1977:10: error[invalid-return-type] Return type does not match returned value: expected `tuple[Unknown, Unknown, Unknown, Unknown]`, found `tuple[@Todo, Unknown]`
+ jax/experimental/mosaic/gpu/utils.py:1977:10: error[invalid-return-type] Return type does not match returned value: expected `tuple[Unknown, Unknown, Unknown, Unknown]`, found `tuple[@Todo(StarredExpression), Unknown]`
- jax/experimental/pallas/ops/gpu/all_gather_mgpu.py:91:6: error[unsupported-operator] Operator `%` is unsupported between objects of type `@Todo | int` and `int | None`
+ jax/experimental/pallas/ops/gpu/all_gather_mgpu.py:91:6: error[unsupported-operator] Operator `%` is unsupported between objects of type `@Todo(StarredExpression) | int` and `int | None`

ibis (https://github.com/ibis-project/ibis)
- ibis/expr/types/arrays.py:226:32: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Value[Array[Unknown], Any], ...]`, found `tuple[Self@concat, ArrayValue, @Todo]`
+ ibis/expr/types/arrays.py:226:32: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Value[Array[Unknown], Any], ...]`, found `tuple[Self@concat, ArrayValue, @Todo(StarredExpression)]`
- ibis/expr/types/arrays.py:1014:29: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Value[Array[Unknown], Any], ...]`, found `tuple[Self@zip, ArrayValue, @Todo]`
+ ibis/expr/types/arrays.py:1014:29: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Value[Array[Unknown], Any], ...]`, found `tuple[Self@zip, ArrayValue, @Todo(StarredExpression)]`
- ibis/expr/types/generic.py:384:29: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Value[Unknown, Any], ...]`, found `tuple[Self@coalesce, @Todo]`
+ ibis/expr/types/generic.py:384:29: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Value[Unknown, Any], ...]`, found `tuple[Self@coalesce, @Todo(StarredExpression)]`
- ibis/expr/types/strings.py:1580:33: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Value[String, Any], ...]`, found `tuple[Self@concat, str | StringValue, @Todo]`
+ ibis/expr/types/strings.py:1580:33: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Value[String, Any], ...]`, found `tuple[Self@concat, str | StringValue, @Todo(StarredExpression)]`

sympy (https://github.com/sympy/sympy)
- sympy/parsing/latex/__init__.py:117:12: error[index-out-of-bounds] Index 3 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
+ sympy/parsing/latex/__init__.py:117:12: error[index-out-of-bounds] Index 3 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:118:30: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
+ sympy/parsing/latex/__init__.py:118:30: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:120:59: error[index-out-of-bounds] Index 3 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
+ sympy/parsing/latex/__init__.py:120:59: error[index-out-of-bounds] Index 3 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:125:12: error[index-out-of-bounds] Index 7 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
+ sympy/parsing/latex/__init__.py:125:12: error[index-out-of-bounds] Index 7 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:126:30: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
+ sympy/parsing/latex/__init__.py:126:30: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:128:53: error[index-out-of-bounds] Index 3 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
+ sympy/parsing/latex/__init__.py:128:53: error[index-out-of-bounds] Index 3 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:133:43: error[index-out-of-bounds] Index 3 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
+ sympy/parsing/latex/__init__.py:133:43: error[index-out-of-bounds] Index 3 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:135:30: error[index-out-of-bounds] Index 5 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
+ sympy/parsing/latex/__init__.py:135:30: error[index-out-of-bounds] Index 5 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:136:12: error[index-out-of-bounds] Index 7 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
+ sympy/parsing/latex/__init__.py:136:12: error[index-out-of-bounds] Index 7 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:139:37: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
+ sympy/parsing/latex/__init__.py:139:37: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:140:30: error[index-out-of-bounds] Index 6 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
+ sympy/parsing/latex/__init__.py:140:30: error[index-out-of-bounds] Index 6 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:140:47: error[index-out-of-bounds] Index 4 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
+ sympy/parsing/latex/__init__.py:140:47: error[index-out-of-bounds] Index 4 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:141:65: error[index-out-of-bounds] Index 5 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
+ sympy/parsing/latex/__init__.py:141:65: error[index-out-of-bounds] Index 5 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/evm/decoding/thegraph/balances.py:139:43: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[ChecksumAddress, ChecksumAddress, ChecksumAddress, int]`, found `tuple[@Todo, Unknown]`
+ rotkehlchen/chain/evm/decoding/thegraph/balances.py:139:43: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[ChecksumAddress, ChecksumAddress, ChecksumAddress, int]`, found `tuple[@Todo(StarredExpression), Unknown]`

core (https://github.com/home-assistant/core)
- homeassistant/components/esphome/light.py:337:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int]`, found `tuple[@Todo, Unknown]`
+ homeassistant/components/esphome/light.py:337:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int]`, found `tuple[@Todo(StarredExpression), Unknown]`
- homeassistant/components/esphome/light.py:358:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int, int]`, found `tuple[@Todo, Unknown, Unknown]`
+ homeassistant/components/esphome/light.py:358:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int, int]`, found `tuple[@Todo(StarredExpression), Unknown, Unknown]`
- homeassistant/components/esphome/light.py:363:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int, int]`, found `tuple[@Todo, Unknown, Unknown]`
+ homeassistant/components/esphome/light.py:363:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int, int]`, found `tuple[@Todo(StarredExpression), Unknown, Unknown]`
- homeassistant/components/flux_led/util.py:98:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int]`, found `tuple[@Todo, int]`
+ homeassistant/components/flux_led/util.py:98:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int]`, found `tuple[@Todo(StarredExpression), int]`
- homeassistant/components/flux_led/util.py:99:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int]`, found `tuple[@Todo, int]`
+ homeassistant/components/flux_led/util.py:99:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int]`, found `tuple[@Todo(StarredExpression), int]`
- homeassistant/components/flux_led/util.py:114:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int, int]`, found `tuple[@Todo, int, int]`
+ homeassistant/components/flux_led/util.py:114:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int, int]`, found `tuple[@Todo(StarredExpression), int, int]`
- homeassistant/components/flux_led/util.py:115:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int, int]`, found `tuple[@Todo, int, int]`
+ homeassistant/components/flux_led/util.py:115:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int, int]`, found `tuple[@Todo(StarredExpression), int, int]`
- homeassistant/components/knx/light.py:375:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int] | None`, found `tuple[@Todo, Unknown & ~None]`
+ homeassistant/components/knx/light.py:375:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int] | None`, found `tuple[@Todo(StarredExpression), Unknown & ~None]`
- homeassistant/components/shelly/light.py:229:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int]`, found `tuple[@Todo, int]`
+ homeassistant/components/shelly/light.py:229:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int]`, found `tuple[@Todo(StarredExpression), int]`
- homeassistant/components/shelly/light.py:404:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int]`, found `tuple[@Todo, Unknown]`
+ homeassistant/components/shelly/light.py:404:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int]`, found `tuple[@Todo(StarredExpression), Unknown]`

egglog-python (https://github.com/egraphs-good/egglog-python)
- python/egglog/builtins.py:1073:31: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
- python/egglog/thunk.py:36:13: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
- python/egglog/thunk.py:83:18: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
- Found 1493 diagnostics
+ Found 1490 diagnostics

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


```

</details>


No memory usage changes detected ✅



---

_Comment by @AlexWaygood on 2025-12-01 11:09_

The conformance-suite diff looks good. Lots of false positives going away. There are a few lines marked as `# E` where we previously emitted diagnostics and no longer do, but in all those cases we were emitting a diagnostic for the wrong reasons.

---

_Marked ready for review by @AlexWaygood on 2025-12-01 11:09_

---

_Review requested from @carljm by @AlexWaygood on 2025-12-01 11:09_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-01 11:09_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-01 11:09_

---

_Comment by @AlexWaygood on 2025-12-01 11:16_

The primer report shows some false positives going away on scipy-stubs and egglog-python. It also features a bunch of pre-existing false positives that have their error messages change. Note that I have a [WIP PR](https://github.com/astral-sh/ruff/pull/21499) that will reduce the places where we infer a `Todo` type for starred expressions, and clear up some of those other pre-existing false positives. But that PR's not ready for review yet.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:8867 on 2025-12-01 13:17_

Not doing this anymore leads to an inconsistent display of various todo types in release mode, but I guess it doesn't hurt to have the one with the message in release builds as well (for these very special todo types)

---

_@sharkdp approved on 2025-12-01 13:21_

Thank you!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:8867 on 2025-12-01 13:23_

yes. One of the criticisms of `Todo` types in the past has been that we don't tell the user what exactly is still to be done if ty has been built in release mode. It seems like a feature that we can show that information for at least _some_ TODOs 😄

---

_@AlexWaygood reviewed on 2025-12-01 13:23_

---

_Merged by @AlexWaygood on 2025-12-01 13:24_

---

_Closed by @AlexWaygood on 2025-12-01 13:24_

---

_Branch deleted on 2025-12-01 13:24_

---
