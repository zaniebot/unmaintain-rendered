```yaml
number: 21840
title: "[ty] improve bad specialization results & error messages"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: specialize-non-generic
created_at: 2025-12-08T07:49:14Z
updated_at: 2025-12-12T03:46:42Z
url: https://github.com/astral-sh/ruff/pull/21840
synced_at: 2026-01-12T15:57:35Z
```

# [ty] improve bad specialization results & error messages

---

_@mtshiba_

## Summary

This PR includes the following changes:

* When attempting to specialize a non-generic type (or a type that is already specialized), the result is `Unknown`. Also, the error message is improved.
* When an implicit type alias is incorrectly specialized, the result is `Unknown`. Also, the error message is improved.
* When only some of the type alias bounds and constraints are not satisfied, not all substitutions are `Unknown`.
* Double specialization is prohibited. e.g. `G[int][int]`

Furthermore, after applying this PR, the fuzzing tests for seeds 1052 and 4419, which panic in main, now pass.
This is because the false recursions on type variables have been removed.

```python
# name_2[0] => Unknown
class name_1[name_2: name_2[0]]:
    def name_4(name_3: name_2, /):
        if name_3:
            pass

#  (name_5 if unique_name_0 else name_1)[0] => Unknown
def name_4[name_5: (name_5 if unique_name_0 else name_1)[0], **name_1](): ...
```

## Test Plan

New corpus test
mdtest files updated


---

_Comment by @astral-sh-bot[bot] on 2025-12-08 07:51_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-12 01:42:35.489686989 +0000
+++ new-output.txt	2025-12-12 01:42:39.236721056 +0000
@@ -4,16 +4,16 @@
 _directives_deprecated_library.py:41:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | float`
 _directives_deprecated_library.py:45:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 aliases_explicit.py:57:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `Unknown`
-aliases_explicit.py:67:24: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
-aliases_explicit.py:68:24: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+aliases_explicit.py:67:9: error[non-subscriptable] Cannot subscript non-generic type
+aliases_explicit.py:68:9: error[non-subscriptable] Cannot subscript non-generic type: `<class 'list[int | None]'>` is already specialized
 aliases_explicit.py:69:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
 aliases_explicit.py:70:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
 aliases_explicit.py:71:24: error[invalid-type-arguments] Type argument for `ParamSpec` must be either a list of types, `ParamSpec`, `Concatenate`, or `...`
 aliases_explicit.py:101:6: error[call-non-callable] Object of type `UnionType` is not callable
-aliases_explicit.py:102:20: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+aliases_explicit.py:102:5: error[non-subscriptable] Cannot subscript non-generic type
 aliases_implicit.py:68:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `Unknown`
-aliases_implicit.py:76:24: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
-aliases_implicit.py:77:24: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+aliases_implicit.py:76:9: error[non-subscriptable] Cannot subscript non-generic type
+aliases_implicit.py:77:9: error[non-subscriptable] Cannot subscript non-generic type: `<class 'list[int | None]'>` is already specialized
 aliases_implicit.py:78:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
 aliases_implicit.py:79:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
 aliases_implicit.py:80:24: error[invalid-type-arguments] Type argument for `ParamSpec` must be either a list of types, `ParamSpec`, `Concatenate`, or `...`
@@ -28,7 +28,7 @@
 aliases_implicit.py:118:10: error[invalid-type-form] Variable of type `Literal["int"]` is not allowed in a type expression
 aliases_implicit.py:119:10: error[invalid-type-form] Variable of type `Literal["int | str"]` is not allowed in a type expression
 aliases_implicit.py:133:6: error[call-non-callable] Object of type `UnionType` is not callable
-aliases_implicit.py:135:20: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+aliases_implicit.py:135:5: error[non-subscriptable] Cannot subscript non-generic type
 aliases_newtype.py:11:8: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["user"]`
 aliases_newtype.py:12:14: error[invalid-assignment] Object of type `Literal[42]` is not assignable to `UserId`
 aliases_newtype.py:18:11: error[invalid-assignment] Object of type `<NewType pseudo-class 'UserId'>` is not assignable to `type`
@@ -606,40 +606,41 @@
 generics_typevartuple_callable.py:49:1: error[type-assertion-failure] Type `tuple[int | float, str, int | float | complex]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_concat.py:47:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_concat.py:52:1: error[type-assertion-failure] Type `tuple[int, bool, str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_specialization.py:45:23: error[invalid-type-arguments] Too many type arguments: expected 0, got 2
-generics_typevartuple_specialization.py:45:51: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
-generics_typevartuple_specialization.py:46:5: error[type-assertion-failure] Type `tuple[int, int | float, bool]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
+generics_typevartuple_specialization.py:45:14: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:45:40: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[str, @Todo(specialized non-generic class)]'>` is already specialized
+generics_typevartuple_specialization.py:46:5: error[type-assertion-failure] Type `tuple[int, int | float, bool]` does not match asserted type `Unknown`
+generics_typevartuple_specialization.py:47:5: error[type-assertion-failure] Type `tuple[str, @Todo(specialized non-generic class)]` does not match asserted type `Unknown`
 generics_typevartuple_specialization.py:51:5: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_specialization.py:52:37: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
-generics_typevartuple_specialization.py:92:28: error[invalid-type-arguments] Too many type arguments: expected 0, got 2
-generics_typevartuple_specialization.py:92:56: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
-generics_typevartuple_specialization.py:93:5: error[type-assertion-failure] Type `tuple[str, int]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_specialization.py:94:5: error[type-assertion-failure] Type `tuple[int | float]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
+generics_typevartuple_specialization.py:92:14: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:92:42: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:93:5: error[type-assertion-failure] Type `tuple[str, int]` does not match asserted type `Unknown`
+generics_typevartuple_specialization.py:94:5: error[type-assertion-failure] Type `tuple[int | float]` does not match asserted type `Unknown`
 generics_typevartuple_specialization.py:95:5: error[type-assertion-failure] Type `tuple[Any, *tuple[Any, ...]]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_specialization.py:102:32: error[invalid-type-arguments] Too many type arguments: expected 0, got 2
-generics_typevartuple_specialization.py:103:33: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
-generics_typevartuple_specialization.py:127:9: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
-generics_typevartuple_specialization.py:130:18: error[invalid-type-arguments] Too many type arguments: expected 0, got 3
+generics_typevartuple_specialization.py:102:20: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:103:21: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:127:5: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:130:14: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
 generics_typevartuple_specialization.py:130:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[tuple[@Todo(PEP 646), ...], T1@func7, T2@func7]`
-generics_typevartuple_specialization.py:134:18: error[invalid-type-arguments] Too many type arguments: expected 0, got 2
-generics_typevartuple_specialization.py:134:37: error[invalid-type-arguments] Too many type arguments: expected 0, got 3
-generics_typevartuple_specialization.py:134:63: error[invalid-type-arguments] Too many type arguments: expected 0, got 4
+generics_typevartuple_specialization.py:134:14: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:134:33: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:134:59: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
 generics_typevartuple_specialization.py:135:5: error[type-assertion-failure] Type `tuple[tuple[()], str, bool]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown]`
 generics_typevartuple_specialization.py:136:5: error[type-assertion-failure] Type `tuple[tuple[str], bool, int | float]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown]`
 generics_typevartuple_specialization.py:137:5: error[type-assertion-failure] Type `tuple[tuple[str, bool], int | float, int]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown]`
-generics_typevartuple_specialization.py:143:18: error[invalid-type-arguments] Too many type arguments: expected 0, got 4
+generics_typevartuple_specialization.py:143:14: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
 generics_typevartuple_specialization.py:143:39: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[tuple[@Todo(PEP 646), ...], T1@func9, T2@func9, T3@func9]`
-generics_typevartuple_specialization.py:147:19: error[invalid-type-arguments] Too many type arguments: expected 0, got 3
-generics_typevartuple_specialization.py:147:45: error[invalid-type-arguments] Too many type arguments: expected 0, got 4
+generics_typevartuple_specialization.py:147:15: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:147:41: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
 generics_typevartuple_specialization.py:148:5: error[type-assertion-failure] Type `tuple[tuple[()], str, bool, int | float]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown, Unknown]`
 generics_typevartuple_specialization.py:149:5: error[type-assertion-failure] Type `tuple[tuple[bool], str, int | float, int]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown, Unknown]`
-generics_typevartuple_specialization.py:153:12: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
-generics_typevartuple_specialization.py:156:28: error[invalid-type-arguments] Too many type arguments: expected 0, got 2
-generics_typevartuple_specialization.py:156:59: error[invalid-type-arguments] Too many type arguments: expected 0, got 2
-generics_typevartuple_specialization.py:157:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], int]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_specialization.py:158:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_specialization.py:159:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_specialization.py:163:13: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+generics_typevartuple_specialization.py:153:8: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:156:24: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:156:55: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:157:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], int]` does not match asserted type `Unknown`
+generics_typevartuple_specialization.py:158:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `Unknown`
+generics_typevartuple_specialization.py:159:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `Unknown`
+generics_typevartuple_specialization.py:163:8: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
 generics_upper_bound.py:37:1: error[type-assertion-failure] Type `list[int]` does not match asserted type `list[Unknown | int]`
 generics_upper_bound.py:38:1: error[type-assertion-failure] Type `set[int]` does not match asserted type `set[Unknown | int]`
 generics_upper_bound.py:43:1: error[type-assertion-failure] Type `list[int] | set[int]` does not match asserted type `list[Unknown | int] | set[Unknown | int]`
@@ -1021,4 +1022,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1023 diagnostics
+Found 1024 diagnostics

```

</details>




---

_Label `ty` added by @AlexWaygood on 2025-12-08 07:52_

---

_Comment by @astral-sh-bot[bot] on 2025-12-08 07:53_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy (https://github.com/python/mypy)
- mypy/typeshed/stdlib/inspect.pyi:192:73: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- mypy/typeshed/stdlib/inspect.pyi:200:84: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+ mypy/typeshed/stdlib/inspect.pyi:192:43: error[non-subscriptable] Cannot subscript non-generic type
+ mypy/typeshed/stdlib/inspect.pyi:200:54: error[non-subscriptable] Cannot subscript non-generic type

koda-validate (https://github.com/keithasaurus/koda-validate)
- koda_validate/dictionary.py:294:34: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:309:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:310:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:326:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:327:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:328:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:344:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:345:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:346:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:347:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:363:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:364:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:365:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:366:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:367:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:383:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:384:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:385:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:386:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:387:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:388:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:404:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:405:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:406:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:407:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:408:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:409:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:410:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:426:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:427:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:428:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:429:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:430:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:431:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:432:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:433:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:449:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:450:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:451:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:452:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:453:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:454:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:455:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:456:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:457:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:473:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:474:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:475:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:476:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:477:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:478:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:479:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:480:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:481:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:482:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:498:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:499:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:500:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:501:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:502:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:503:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:504:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:505:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:506:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:507:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:508:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:524:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:525:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:526:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:527:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:528:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:529:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:530:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:531:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:532:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:533:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:534:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:535:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:551:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:552:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:553:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:554:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:555:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:556:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:557:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:558:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:559:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:560:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:561:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:562:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:563:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:581:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:582:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:583:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:584:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:585:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:586:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:587:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:588:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:589:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:590:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:591:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:592:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:593:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:594:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:612:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:613:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:614:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:615:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:616:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:617:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:618:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:619:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:620:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:621:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:622:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:623:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:624:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:625:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:626:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:644:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:645:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:646:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:647:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:648:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:649:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:650:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:651:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:652:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:653:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:654:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:655:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:656:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:657:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:658:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:659:26: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:695:32: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:696:32: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:696:50: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:697:32: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:697:50: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:697:68: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:698:32: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:698:50: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:698:68: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:698:86: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:700:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:701:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:702:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:703:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:704:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:707:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:708:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:709:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:710:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:711:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:712:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:715:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:716:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:717:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:718:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:719:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:720:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:721:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:724:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:725:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:726:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:727:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:728:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:729:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:730:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:731:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:734:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:735:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:736:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:737:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:738:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:739:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:740:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:741:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:742:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:745:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:746:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:747:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:748:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:749:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:750:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:751:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:752:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:753:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:754:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:757:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:758:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:759:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:760:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:761:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:762:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:763:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:764:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:765:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:766:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:767:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:770:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:771:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:772:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:773:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:774:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:775:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:776:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:777:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:778:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:779:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:780:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:781:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:784:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:785:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:786:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:787:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:788:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:789:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:790:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:791:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:792:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:793:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:794:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:795:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:796:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:799:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:800:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:801:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:802:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:803:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:804:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:805:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:806:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:807:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:808:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:809:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:810:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:811:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:812:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:815:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:816:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:817:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:818:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:819:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:820:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:821:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:822:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:823:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:824:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:825:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:826:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:827:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:828:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:829:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:832:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:833:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:834:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:835:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:836:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:837:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:838:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:839:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:840:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:841:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:842:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:843:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:844:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:845:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:846:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:847:30: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
- koda_validate/dictionary.py:858:39: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+ koda_validate/dictionary.py:294:21: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:309:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:310:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:326:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:327:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:328:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:344:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:345:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:346:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:347:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:363:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:364:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:365:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:366:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:367:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:383:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:384:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:385:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:386:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:387:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:388:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:404:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:405:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:406:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:407:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:408:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:409:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:410:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:426:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:427:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:428:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:429:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:430:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:431:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:432:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:433:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:449:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:450:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:451:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:452:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:453:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:454:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:455:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:456:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:457:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:473:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:474:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:475:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:476:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:477:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:478:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:479:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:480:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:481:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:482:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:498:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:499:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:500:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:501:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:502:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:503:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:504:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:505:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:506:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:507:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:508:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:524:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:525:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:526:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:527:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:528:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:529:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:530:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:531:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:532:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:533:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:534:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:535:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:551:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:552:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:553:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:554:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:555:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:556:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:557:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:558:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:559:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:560:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:561:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:562:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:563:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:581:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:582:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:583:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:584:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:585:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:586:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:587:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:588:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:589:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:590:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:591:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:592:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:593:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:594:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:612:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:613:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:614:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator[A@KeyValidator]]'>` is already specialized
+ koda_validate/dictionary.py:615:13: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Hashable, Validator

... (truncated 615 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/generics/pep695/aliases.md`:187 on 2025-12-08 07:55_

This behavior is consistent with pyright.
mypy and pyrefly do not replace even the failed parts with `Unknown`, which is not a good idea.

---

_@mtshiba reviewed on 2025-12-08 07:56_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-08 08:10_

---

_Comment by @mtshiba on 2025-12-08 08:14_

The type conformance test results are good: the error messages are mostly as written in the scripts (except for `TypeVarTuple`).

---

_Comment by @astral-sh-bot[bot] on 2025-12-08 08:19_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-type-arguments` | 0 | 460 | 2 |
| `non-subscriptable` | 460 | 0 | 0 |
| `unresolved-attribute` | 0 | 0 | 7 |
| `invalid-argument-type` | 0 | 2 | 0 |
| `type-assertion-failure` | 2 | 0 | 0 |
| `unused-ignore-comment` | 2 | 0 | 0 |
| **Total** | **464** | **462** | **9** |




---

_Marked ready for review by @mtshiba on 2025-12-08 08:38_

---

_Review requested from @carljm by @mtshiba on 2025-12-08 08:38_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-12-08 08:38_

---

_Review requested from @sharkdp by @mtshiba on 2025-12-08 08:38_

---

_Review requested from @dcreager by @mtshiba on 2025-12-08 08:38_

---

_@AlexWaygood reviewed on 2025-12-08 14:44_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/corpus/cyclic_pep695_typevars.py`:12 on 2025-12-08 14:44_

I'd prefer it if we could put each minimal repro in a separate file, so that we can be confident that they're not "interfering" with each other

---

_Comment by @AlexWaygood on 2025-12-08 14:44_

I find some of the error messages on this branch a bit confusing. For example:

```py
class Foo[T]:
    x: T

# error[non-subscriptable] "Cannot subscript non-generic type alias: `<class 'Foo[int]'>` is already specialized"
f = Foo[int][str]()
```

But I never created a type alias there!

---

_@AlexWaygood reviewed on 2025-12-08 14:52_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:98 on 2025-12-08 14:52_

this feels suspiciously specific ;)

What about doubly specialized `Protocol` instances, doubly specialized `TypedDict` instances, and doubly specialized `tuple` instances? Don't they also deserve to have the "<NAME> is already specialized" part of the diagnostic message?

```py
from typing import Protocol, TypedDict

class Foo[T](Protocol):
    x: T

type Bar = Foo[int]

class Spam[T](TypedDict):
    x: T

type Ham = Spam[int]

b: Bar[str]  # error: "Cannot subscript non-generic type alias"
h: Ham[int]  # error: "Cannot subscript non-generic type alias"

type SpecializedTuple = tuple[int, int]
DoubleSpecialized = SpecializedTuple[str]  # error: "Cannot subscript non-generic type alias"
```

---

_@AlexWaygood reviewed on 2025-12-08 14:59_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:11825 on 2025-12-08 14:59_

you could have the bit of the message after the colon as the primary annotation message. It will result in the same concise diagnostic message, but the verbose diagnostic will be rendered in a prettier way. E.g. if I apply this diff to your branch:

```diff
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index a096882993..4a013dd4ef 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -11818,15 +11818,13 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
                     .context
                     .report_lint(&NON_SUBSCRIPTABLE, &*subscript.value)
                 {
+                    let mut diagnostic =
+                        builder.into_diagnostic("Cannot subscript non-generic type alias");
                     if value_ty.is_generic_alias() {
-                        builder.into_diagnostic(format_args!(
-                            "Cannot subscript non-generic type alias: `{}` is already specialized",
+                        diagnostic.set_primary_message(format_args!(
+                            "`{}` is already specialized",
                             value_ty.display(db),
                         ));
-                    } else {
-                        builder.into_diagnostic(format_args!(
-                            "Cannot subscript non-generic type alias"
-                        ));
                     }
                 }
                 error = Some(ExplicitSpecializationError::NonGeneric);
@@ -12072,15 +12070,13 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
                 debug_assert!(alias.specialization(db).is_none());
                 if let Some(builder) = self.context.report_lint(&NON_SUBSCRIPTABLE, subscript) {
                     let value_type = alias.raw_value_type(db);
+                    let mut diagnostic =
+                        builder.into_diagnostic("Cannot subscript non-generic type alias");
                     if value_type.is_generic_nominal_instance() {
-                        builder.into_diagnostic(format_args!(
-                            "Cannot subscript non-generic type alias: `{}` is already specialized",
+                        diagnostic.set_primary_message(format_args!(
+                            "`{}` is already specialized",
                             value_type.display(db),
                         ));
-                    } else {
-                        builder.into_diagnostic(format_args!(
-                            "Cannot subscript non-generic type alias"
-                        ));
                     }
                 }
```

then the diagnostics look like this:

<img width="1232" height="294" alt="image" src="https://github.com/user-attachments/assets/50405a49-076d-48f8-8a71-46d3d265d744" />

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:11823 on 2025-12-08 18:16_

(Addressing another of Alex's comments): I think we should just remove the phrase "type alias" from this message, so it just says "Cannot subscript non-generic".

Part of the confusion here is that we (and the runtime `typing` module) use the term "generic alias" for an explicit specialization of a generic class, e.g. `Foo[int]` where `class Foo[T]: ...`. But a "generic alias" is not a "type alias".

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:11920 on 2025-12-08 18:27_

This behavior change fits well with a TODO I recently added to consider lazily checking explicit specializations against bounds/constraints, instead of checking it eagerly. (If we are going to return the requested specialization here even if it violates bounds/constraints, then there is no need to eagerly evaluate the bounds/constraints at all.) This would help us avoid some cycle problems.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:12084 on 2025-12-08 18:30_

As Alex points out in separate comment, `is_generic_nominal_instance` is awkwardly specific -- probably too specific.

Would there be any harm in just always providing the actual type we failed to specialize? Why not simplify to
```suggestion
                    builder.into_diagnostic(format_args!(
                          "Cannot subscript non-generic type `{}`",
                          value_type.display(db),
                      ));
                    
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:11823 on 2025-12-08 18:31_

Similar to below, I'm not really clear why we should only provide `value_ty` in the diagnostic in certain limited cases. Why not simplify this code and just always provide it, with a message like "Cannot subscript non-generic type `{}`"? To me that seems clear enough in all cases.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:961 on 2025-12-08 18:32_

Similar comment as below

---

_@carljm reviewed on 2025-12-08 18:33_

This looks excellent, thank you!

I only have minor nits about the diagnostic messages.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/infer/builder.rs`:11726 on 2025-12-10 05:08_

I think this should use `invalid-type-arguments` instead as it's not related to the definition of a `ParamSpec` but rather the specialization of it.

---

_@dhruvmanila reviewed on 2025-12-10 05:10_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:98 on 2025-12-10 10:20_

This method no longer seems like it belongs in `instance.rs`; it's not specific to `Type::NominalInstance` anymore

---

_@AlexWaygood reviewed on 2025-12-10 10:20_

---

_@mtshiba reviewed on 2025-12-10 10:21_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer/builder.rs`:12084 on 2025-12-10 10:21_

Consider the case where the user has a type alias `type MyList = list[int]` and accidentally specifies `MyList[int]`.
The user will see an error "Cannot subscript non-generic type `list[int]`", which is likely to be confusing. If the user mistakenly thought `MyList` was a generic type and wanted `list[int]` as the result of specialization, the user might not immediately realize what the problem is. In fact, pyright gives a hint in this case: "`list[int]` is already specialized".

https://pyright-play.net/?pythonVersion=3.12&code=C4TwDgpgBAsiCSA7YUC8UCWyCwAoPAJhAGZQD6AFBgFywLIDaWwAugJTV5TdQBOEANwgBDADZlQkKmzx5J0OABkMAZxTpRq4E2QtZuIqUqjaSrTtYcuPfkLETwECqJm4gA

Also, the error message should be mostly the same whether the type alias is defined implicitly or using PEP 695 syntax.


---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:11920 on 2025-12-10 22:56_

Actually I misread this on my first review -- you are still replacing invalid-for-bound/constraints specializations with `Unknown` (by pushing `Unknown` onto `specialization_types` above), so this doesn't lend itself to the behavior change I was considering.

---

_Label `ecosystem-analyzer` removed by @carljm on 2025-12-10 23:04_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-10 23:04_

---

_Review requested from @carljm by @mtshiba on 2025-12-12 02:16_

---

_@carljm approved on 2025-12-12 03:21_

Looks good, thank you!

---

_Merged by @carljm on 2025-12-12 03:21_

---

_Closed by @carljm on 2025-12-12 03:21_

---

_Branch deleted on 2025-12-12 03:46_

---
