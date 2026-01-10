```yaml
number: 22193
title: "[ty] Rename `non-subscriptable` to `not-subscriptable`"
type: pull_request
state: merged
author: silamon
labels:
  - breaking
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: rep-not-subscriptable
created_at: 2025-12-25T09:30:41Z
updated_at: 2025-12-29T20:02:36Z
url: https://github.com/astral-sh/ruff/pull/22193
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Rename `non-subscriptable` to `not-subscriptable`

---

_Pull request opened by @silamon on 2025-12-25 09:30_

## Summary

Fixes https://github.com/astral-sh/ty/issues/2201 by renaming the diagnostic

## Test Plan

Existing tests


---

_Review requested from @carljm by @silamon on 2025-12-25 09:30_

---

_Review requested from @AlexWaygood by @silamon on 2025-12-25 09:30_

---

_Review requested from @sharkdp by @silamon on 2025-12-25 09:30_

---

_Review requested from @dcreager by @silamon on 2025-12-25 09:30_

---

_Review requested from @MichaReiser by @silamon on 2025-12-25 09:30_

---

_Renamed from "[ty] Replace `non-subscriptable` to `not-subscriptable`" to "[ty] Rename `non-subscriptable` to `not-subscriptable`" by @silamon on 2025-12-25 09:31_

---

_Comment by @astral-sh-bot[bot] on 2025-12-25 09:32_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-27 11:41:58.404956431 +0000
+++ new-output.txt	2025-12-27 11:42:01.873967488 +0000
@@ -4,16 +4,16 @@
 _directives_deprecated_library.py:41:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | float`
 _directives_deprecated_library.py:45:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 aliases_explicit.py:57:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `Unknown`
-aliases_explicit.py:67:9: error[non-subscriptable] Cannot subscript non-generic type
-aliases_explicit.py:68:9: error[non-subscriptable] Cannot subscript non-generic type: `<class 'list[int | None]'>` is already specialized
+aliases_explicit.py:67:9: error[not-subscriptable] Cannot subscript non-generic type
+aliases_explicit.py:68:9: error[not-subscriptable] Cannot subscript non-generic type: `<class 'list[int | None]'>` is already specialized
 aliases_explicit.py:69:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
 aliases_explicit.py:70:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
 aliases_explicit.py:71:24: error[invalid-type-arguments] Type argument for `ParamSpec` must be either a list of types, `ParamSpec`, `Concatenate`, or `...`
 aliases_explicit.py:101:6: error[call-non-callable] Object of type `UnionType` is not callable
-aliases_explicit.py:102:5: error[non-subscriptable] Cannot subscript non-generic type
+aliases_explicit.py:102:5: error[not-subscriptable] Cannot subscript non-generic type
 aliases_implicit.py:68:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `Unknown`
-aliases_implicit.py:76:9: error[non-subscriptable] Cannot subscript non-generic type
-aliases_implicit.py:77:9: error[non-subscriptable] Cannot subscript non-generic type: `<class 'list[int | None]'>` is already specialized
+aliases_implicit.py:76:9: error[not-subscriptable] Cannot subscript non-generic type
+aliases_implicit.py:77:9: error[not-subscriptable] Cannot subscript non-generic type: `<class 'list[int | None]'>` is already specialized
 aliases_implicit.py:78:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
 aliases_implicit.py:79:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
 aliases_implicit.py:80:24: error[invalid-type-arguments] Type argument for `ParamSpec` must be either a list of types, `ParamSpec`, `Concatenate`, or `...`
@@ -28,7 +28,7 @@
 aliases_implicit.py:118:10: error[invalid-type-form] Variable of type `Literal["int"]` is not allowed in a type expression
 aliases_implicit.py:119:10: error[invalid-type-form] Variable of type `Literal["int | str"]` is not allowed in a type expression
 aliases_implicit.py:133:6: error[call-non-callable] Object of type `UnionType` is not callable
-aliases_implicit.py:135:5: error[non-subscriptable] Cannot subscript non-generic type
+aliases_implicit.py:135:5: error[not-subscriptable] Cannot subscript non-generic type
 aliases_newtype.py:11:8: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["user"]`
 aliases_newtype.py:12:14: error[invalid-assignment] Object of type `Literal[42]` is not assignable to `UserId`
 aliases_newtype.py:18:11: error[invalid-assignment] Object of type `<NewType pseudo-class 'UserId'>` is not assignable to `type`
@@ -614,41 +614,41 @@
 generics_typevartuple_callable.py:49:1: error[type-assertion-failure] Type `tuple[int | float, str, int | float | complex]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_concat.py:47:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_concat.py:52:1: error[type-assertion-failure] Type `tuple[int, bool, str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_specialization.py:45:14: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
-generics_typevartuple_specialization.py:45:40: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[str, @Todo(specialized non-generic class)]'>` is already specialized
+generics_typevartuple_specialization.py:45:14: error[not-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:45:40: error[not-subscriptable] Cannot subscript non-generic type: `<class 'tuple[str, @Todo(specialized non-generic class)]'>` is already specialized
 generics_typevartuple_specialization.py:46:5: error[type-assertion-failure] Type `tuple[int, int | float, bool]` does not match asserted type `Unknown`
 generics_typevartuple_specialization.py:47:5: error[type-assertion-failure] Type `tuple[str, @Todo(specialized non-generic class)]` does not match asserted type `Unknown`
 generics_typevartuple_specialization.py:51:5: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_specialization.py:52:37: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
-generics_typevartuple_specialization.py:92:14: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
-generics_typevartuple_specialization.py:92:42: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:92:14: error[not-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:92:42: error[not-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
 generics_typevartuple_specialization.py:93:5: error[type-assertion-failure] Type `tuple[str, int]` does not match asserted type `Unknown`
 generics_typevartuple_specialization.py:94:5: error[type-assertion-failure] Type `tuple[int | float]` does not match asserted type `Unknown`
 generics_typevartuple_specialization.py:95:5: error[type-assertion-failure] Type `tuple[Any, *tuple[Any, ...]]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_specialization.py:102:20: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
-generics_typevartuple_specialization.py:103:21: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
-generics_typevartuple_specialization.py:127:5: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
-generics_typevartuple_specialization.py:130:14: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:102:20: error[not-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:103:21: error[not-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:127:5: error[not-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:130:14: error[not-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
 generics_typevartuple_specialization.py:130:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[tuple[@Todo(PEP 646), ...], T1@func7, T2@func7]`
-generics_typevartuple_specialization.py:134:14: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
-generics_typevartuple_specialization.py:134:33: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
-generics_typevartuple_specialization.py:134:59: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:134:14: error[not-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:134:33: error[not-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:134:59: error[not-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
 generics_typevartuple_specialization.py:135:5: error[type-assertion-failure] Type `tuple[tuple[()], str, bool]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown]`
 generics_typevartuple_specialization.py:136:5: error[type-assertion-failure] Type `tuple[tuple[str], bool, int | float]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown]`
 generics_typevartuple_specialization.py:137:5: error[type-assertion-failure] Type `tuple[tuple[str, bool], int | float, int]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown]`
-generics_typevartuple_specialization.py:143:14: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:143:14: error[not-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
 generics_typevartuple_specialization.py:143:39: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[tuple[@Todo(PEP 646), ...], T1@func9, T2@func9, T3@func9]`
-generics_typevartuple_specialization.py:147:15: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
-generics_typevartuple_specialization.py:147:41: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:147:15: error[not-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:147:41: error[not-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
 generics_typevartuple_specialization.py:148:5: error[type-assertion-failure] Type `tuple[tuple[()], str, bool, int | float]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown, Unknown]`
 generics_typevartuple_specialization.py:149:5: error[type-assertion-failure] Type `tuple[tuple[bool], str, int | float, int]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown, Unknown]`
-generics_typevartuple_specialization.py:153:8: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
-generics_typevartuple_specialization.py:156:24: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
-generics_typevartuple_specialization.py:156:55: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:153:8: error[not-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:156:24: error[not-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:156:55: error[not-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
 generics_typevartuple_specialization.py:157:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], int]` does not match asserted type `Unknown`
 generics_typevartuple_specialization.py:158:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `Unknown`
 generics_typevartuple_specialization.py:159:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `Unknown`
-generics_typevartuple_specialization.py:163:8: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
+generics_typevartuple_specialization.py:163:8: error[not-subscriptable] Cannot subscript non-generic type: `<class 'tuple[@Todo(PEP 646), ...]'>` is already specialized
 generics_upper_bound.py:37:1: error[type-assertion-failure] Type `list[int]` does not match asserted type `list[Unknown | int]`
 generics_upper_bound.py:38:1: error[type-assertion-failure] Type `set[int]` does not match asserted type `set[Unknown | int]`
 generics_upper_bound.py:43:1: error[type-assertion-failure] Type `list[int] | set[int]` does not match asserted type `list[Unknown | int] | set[Unknown | int]`
@@ -753,7 +753,7 @@
 namedtuples_usage.py:35:7: error[index-out-of-bounds] Index -4 is out of bounds for tuple `Point` with length 3
 namedtuples_usage.py:40:1: error[invalid-assignment] Cannot assign to read-only property `x` on object of type `Point`
 namedtuples_usage.py:41:1: error[invalid-assignment] Cannot assign to a subscript on an object of type `Point`
-namedtuples_usage.py:43:5: error[non-subscriptable] Cannot delete subscript on object of type `Point` with no `__delitem__` method
+namedtuples_usage.py:43:5: error[not-subscriptable] Cannot delete subscript on object of type `Point` with no `__delitem__` method
 namedtuples_usage.py:52:1: error[invalid-assignment] Too many values to unpack: Expected 2
 namedtuples_usage.py:53:1: error[invalid-assignment] Not enough values to unpack: Expected 4
 narrowing_typeguard.py:17:9: error[type-assertion-failure] Type `tuple[str, str]` does not match asserted type `tuple[str, ...]`

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-25 09:34_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
more-itertools (https://github.com/more-itertools/more-itertools)
- more_itertools/more.py:5442:12: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ more_itertools/more.py:5442:12: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- more_itertools/more.py:5444:20: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ more_itertools/more.py:5444:20: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method

attrs (https://github.com/python-attrs/attrs)
- tests/test_make.py:1514:61: error[non-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ tests/test_make.py:1514:61: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
- tests/test_setattr.py:137:29: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_setattr.py:137:29: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/test_setattr.py:138:24: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_setattr.py:138:24: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method

kornia (https://github.com/kornia/kornia)
- kornia/augmentation/random_generator/_3d/affine.py:146:35: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ kornia/augmentation/random_generator/_3d/affine.py:146:35: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- kornia/augmentation/random_generator/_3d/affine.py:147:35: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ kornia/augmentation/random_generator/_3d/affine.py:147:35: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- kornia/augmentation/random_generator/_3d/affine.py:148:35: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ kornia/augmentation/random_generator/_3d/affine.py:148:35: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- kornia/augmentation/random_generator/_3d/affine.py:149:56: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ kornia/augmentation/random_generator/_3d/affine.py:149:56: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- kornia/augmentation/random_generator/_3d/affine.py:149:75: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ kornia/augmentation/random_generator/_3d/affine.py:149:75: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- kornia/augmentation/random_generator/_3d/affine.py:150:56: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ kornia/augmentation/random_generator/_3d/affine.py:150:56: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- kornia/augmentation/random_generator/_3d/affine.py:150:75: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ kornia/augmentation/random_generator/_3d/affine.py:150:75: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- kornia/augmentation/random_generator/_3d/affine.py:151:56: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ kornia/augmentation/random_generator/_3d/affine.py:151:56: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- kornia/augmentation/random_generator/_3d/affine.py:151:75: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ kornia/augmentation/random_generator/_3d/affine.py:151:75: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
- pytest_robotframework/_internal/pytest/plugin.py:235:89: warning[unused-ignore-comment] Unused `ty: ignore` directive
+ pytest_robotframework/_internal/pytest/plugin.py:235:101: warning[ignore-comment-unknown-rule] Unknown rule `non-subscriptable`. Did you mean `not-subscriptable`?

pip (https://github.com/pypa/pip)
- src/pip/_internal/req/req_uninstall.py:102:17: error[non-subscriptable] Cannot subscript object of type `Sized` with no `__getitem__` method
+ src/pip/_internal/req/req_uninstall.py:102:17: error[not-subscriptable] Cannot subscript object of type `Sized` with no `__getitem__` method
- src/pip/_vendor/pygments/lexer.py:838:24: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ src/pip/_vendor/pygments/lexer.py:838:24: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- src/pip/_vendor/pygments/lexer.py:845:43: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ src/pip/_vendor/pygments/lexer.py:845:43: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- src/pip/_vendor/pygments/lexers/python.py:410:26: error[non-subscriptable] Cannot subscript object of type `Self@analyse_text` with no `__getitem__` method
+ src/pip/_vendor/pygments/lexers/python.py:410:26: error[not-subscriptable] Cannot subscript object of type `Self@analyse_text` with no `__getitem__` method
- src/pip/_vendor/pygments/lexers/python.py:1198:17: error[non-subscriptable] Cannot subscript object of type `Self@analyse_text` with no `__getitem__` method
+ src/pip/_vendor/pygments/lexers/python.py:1198:17: error[not-subscriptable] Cannot subscript object of type `Self@analyse_text` with no `__getitem__` method
- src/pip/_vendor/rich/prompt.py:363:25: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ src/pip/_vendor/rich/prompt.py:363:25: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- src/pip/_vendor/urllib3/poolmanager.py:279:22: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ src/pip/_vendor/urllib3/poolmanager.py:279:22: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- src/pip/_vendor/urllib3/poolmanager.py:280:20: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ src/pip/_vendor/urllib3/poolmanager.py:280:20: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- src/pip/_vendor/urllib3/poolmanager.py:281:20: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ src/pip/_vendor/urllib3/poolmanager.py:281:20: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method

stone (https://github.com/dropbox/stone)
- stone/ir/data_types.py:1098:30: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ stone/ir/data_types.py:1098:30: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- stone/ir/data_types.py:1293:19: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ stone/ir/data_types.py:1293:19: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- stone/ir/data_types.py:1346:19: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ stone/ir/data_types.py:1346:19: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- stone/ir/data_types.py:1529:23: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ stone/ir/data_types.py:1529:23: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- test/test_backend.py:273:17: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ test/test_backend.py:273:17: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- test/test_python_types.py:173:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ test/test_python_types.py:173:9: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method

spack (https://github.com/spack/spack)
- lib/spack/spack/ci/__init__.py:914:33: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/ci/__init__.py:914:33: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- lib/spack/spack/ci/__init__.py:960:20: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/ci/__init__.py:960:20: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- lib/spack/spack/ci/__init__.py:969:25: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/ci/__init__.py:969:25: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- lib/spack/spack/ci/__init__.py:1051:20: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/ci/__init__.py:1051:20: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- lib/spack/spack/graph.py:103:16: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/graph.py:103:16: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- lib/spack/spack/graph.py:105:16: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/graph.py:105:16: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- lib/spack/spack/graph.py:106:20: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/graph.py:106:20: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- lib/spack/spack/test/cmd/uninstall.py:126:36: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/test/cmd/uninstall.py:126:36: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- lib/spack/spack/test/cmd/uninstall.py:129:36: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/test/cmd/uninstall.py:129:36: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- lib/spack/spack/test/cmd/uninstall.py:132:36: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/test/cmd/uninstall.py:132:36: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- lib/spack/spack/test/cmd/uninstall.py:135:36: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/test/cmd/uninstall.py:135:36: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- lib/spack/spack/test/config.py:751:26: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/test/config.py:751:26: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- lib/spack/spack/test/spec_syntax.py:1305:12: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/test/spec_syntax.py:1305:12: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- lib/spack/spack/test/spec_syntax.py:1307:12: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/test/spec_syntax.py:1307:12: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- lib/spack/spack/test/spec_syntax.py:1312:12: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/test/spec_syntax.py:1312:12: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- lib/spack/spack/test/spec_syntax.py:1319:12: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/test/spec_syntax.py:1319:12: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- lib/spack/spack/test/spec_syntax.py:1322:12: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/test/spec_syntax.py:1322:12: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- lib/spack/spack/vendor/jsonschema/exceptions.py:268:13: error[non-subscriptable] Cannot subscript object of type `Unset` with no `__getitem__` method
+ lib/spack/spack/vendor/jsonschema/exceptions.py:268:13: error[not-subscriptable] Cannot subscript object of type `Unset` with no `__getitem__` method
- lib/spack/spack/vendor/macholib/MachO.py:400:29: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/vendor/macholib/MachO.py:400:29: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- lib/spack/spack/vendor/ruamel/yaml/emitter.py:1022:26: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/vendor/ruamel/yaml/emitter.py:1022:26: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/http.py:277:18: error[non-subscriptable] Cannot subscript object of type `object` with no `__getitem__` method
+ src/werkzeug/http.py:277:18: error[not-subscriptable] Cannot subscript object of type `object` with no `__getitem__` method
- src/werkzeug/utils.py:111:17: error[non-subscriptable] Cannot delete subscript on object of type `object` with no `__delitem__` method
+ src/werkzeug/utils.py:111:17: error[not-subscriptable] Cannot delete subscript on object of type `object` with no `__delitem__` method
- tests/middleware/test_http_proxy.py:34:12: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/middleware/test_http_proxy.py:34:12: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/middleware/test_http_proxy.py:35:12: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/middleware/test_http_proxy.py:35:12: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/middleware/test_http_proxy.py:36:12: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/middleware/test_http_proxy.py:36:12: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/middleware/test_http_proxy.py:39:12: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/middleware/test_http_proxy.py:39:12: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/middleware/test_http_proxy.py:40:12: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/middleware/test_http_proxy.py:40:12: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/middleware/test_http_proxy.py:41:12: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/middleware/test_http_proxy.py:41:12: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/middleware/test_http_proxy.py:42:12: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/middleware/test_http_proxy.py:42:12: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/middleware/test_http_proxy.py:46:12: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/middleware/test_http_proxy.py:46:12: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/middleware/test_http_proxy.py:47:12: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/middleware/test_http_proxy.py:47:12: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/middleware/test_http_proxy.py:51:12: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/middleware/test_http_proxy.py:51:12: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method

scrapy (https://github.com/scrapy/scrapy)
- tests/test_commands.py:46:27: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_commands.py:46:27: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/test_commands.py:47:21: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_commands.py:47:21: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/test_dependencies.py:26:41: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_dependencies.py:26:41: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/test_loader.py:65:16: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_loader.py:65:16: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method

paasta (https://github.com/yelp/paasta)
- paasta_tools/cli/cmds/mark_for_deployment.py:1311:18: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/cli/cmds/mark_for_deployment.py:1311:18: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/cli/cmds/mark_for_deployment.py:1312:18: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/cli/cmds/mark_for_deployment.py:1312:18: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/cli/cmds/mark_for_deployment.py:1313:22: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/cli/cmds/mark_for_deployment.py:1313:22: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/cli/cmds/spark_run.py:816:26: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/cli/cmds/spark_run.py:816:26: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/cli/cmds/spark_run.py:817:23: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/cli/cmds/spark_run.py:817:23: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/cli/cmds/status.py:852:25: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/cli/cmds/status.py:852:25: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/mesos_tools.py:893:16: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ paasta_tools/mesos_tools.py:893:16: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:629:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:629:9: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:638:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:638:9: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:639:28: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:639:28: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:640:31: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:640:31: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:641:30: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:641:30: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:652:30: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:652:30: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:653:29: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:653:29: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:654:38: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:654:38: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:660:22: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:660:22: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:668:22: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:668:22: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:746:17: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:746:17: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:749:17: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:749:17: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:752:60: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:752:60: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:755:16: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:755:16: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:758:25: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:758:25: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:763:27: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:763:27: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:767:27: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:767:27: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:772:28: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:772:28: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:777:27: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:777:27: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:780:20: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:780:20: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:784:36: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:784:36: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:791:31: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:791:31: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:796:37: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:796:37: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:803:13: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:803:13: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:803:45: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:803:45: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:810:27: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:810:27: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/paastaapi/api_client.py:811:27: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/paastaapi/api_client.py:811:27: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/setup_prometheus_adapter_config.py:819:24: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/setup_prometheus_adapter_config.py:819:24: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/setup_prometheus_adapter_config.py:820:25: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/setup_prometheus_adapter_config.py:820:25: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/setup_prometheus_adapter_config.py:835:22: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/setup_prometheus_adapter_config.py:835:22: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- paasta_tools/tron_tools.py:194:24: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/tron_tools.py:194:24: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method

graphql-core (https://github.com/graphql-python/graphql-core)
- tests/execution/test_resolve.py:322:20: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/execution/test_resolve.py:322:20: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/junitxml.py:125:21: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ src/_pytest/junitxml.py:125:21: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- src/_pytest/junitxml.py:127:12: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ src/_pytest/junitxml.py:127:12: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- src/_pytest/junitxml.py:128:33: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ src/_pytest/junitxml.py:128:33: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- src/_pytest/python.py:951:31: error[non-subscriptable] Cannot subscript object of type `_HiddenParam` with no `__getitem__` method
+ src/_pytest/python.py:951:31: error[not-subscriptable] Cannot subscript object of type `_HiddenParam` with no `__getitem__` method

dulwich (https://github.com/dulwich/dulwich)
- dulwich/midx.py:199:21: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ dulwich/midx.py:199:21: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- dulwich/midx.py:204:24: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ dulwich/midx.py:204:24: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- dulwich/midx.py:209:31: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ dulwich/midx.py:209:31: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- dulwich/midx.py:218:28: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ dulwich/midx.py:218:28: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- dulwich/midx.py:221:32: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ dulwich/midx.py:221:32: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- dulwich/midx.py:226:50: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ dulwich/midx.py:226:50: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- dulwich/midx.py:237:24: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ dulwich/midx.py:237:24: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- dulwich/midx.py:239:23: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ dulwich/midx.py:239:23: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- dulwich/midx.py:281:25: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ dulwich/midx.py:281:25: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- dulwich/midx.py:297:23: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ dulwich/midx.py:297:23: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- dulwich/midx.py:339:28: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ dulwich/midx.py:339:28: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- dulwich/midx.py:356:42: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ dulwich/midx.py:356:42: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- dulwich/midx.py:357:46: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ dulwich/midx.py:357:46: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- dulwich/midx.py:368:23: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ dulwich/midx.py:368:23: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method

kopf (https://github.com/nolar/kopf)
- kopf/_cogs/structs/credentials.py:166:44: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ kopf/_cogs/structs/credentials.py:166:44: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method

alerta (https://github.com/alerta/alerta)
- alerta/plugins/escalate.py:47:34: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ alerta/plugins/escalate.py:47:34: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method

boostedblob (https://github.com/hauntsaninja/boostedblob)
- boostedblob/boost.py:382:19: error[non-subscriptable] Cannot subscript object of type `Collection[Awaitable[T@OrderedMappingBoostable]]` with no `__getitem__` method
+ boostedblob/boost.py:382:19: error[not-subscriptable] Cannot subscript object of type `Collection[Awaitable[T@OrderedMappingBoostable]]` with no `__getitem__` method

rich (https://github.com/Textualize/rich)
- rich/prompt.py:363:25: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ rich/prompt.py:363:25: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method

ignite (https://github.com/pytorch/ignite)
- tests/ignite/conftest.py:125:34: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/conftest.py:125:34: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/conftest.py:125:34: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/conftest.py:125:34: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/distributed/test_launcher.py:281:44: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/ignite/distributed/test_launcher.py:281:44: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/ignite/distributed/test_launcher.py:282:42: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/ignite/distributed/test_launcher.py:282:42: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/ignite/distributed/utils/__init__.py:457:24: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/distributed/utils/__init__.py:457:24: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/distributed/utils/__init__.py:457:24: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/distributed/utils/__init__.py:457:24: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/distributed/utils/__init__.py:459:24: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/distributed/utils/__init__.py:459:24: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/distributed/utils/__init__.py:459:24: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/distributed/utils/__init__.py:459:24: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/distributed/utils/__init__.py:482:24: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/distributed/utils/__init__.py:482:24: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/distributed/utils/__init__.py:482:24: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/distributed/utils/__init__.py:482:24: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/distributed/utils/__init__.py:484:24: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/distributed/utils/__init__.py:484:24: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/distributed/utils/__init__.py:484:24: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/distributed/utils/__init__.py:484:24: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/handlers/test_state_param_scheduler.py:87:19: error[non-subscriptable] Cannot subscript object of type `object` with no `__getitem__` method
+ tests/ignite/handlers/test_state_param_scheduler.py:87:19: error[not-subscriptable] Cannot subscript object of type `object` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:191:12: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:191:12: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:191:12: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:191:12: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:193:12: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:193:12: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:193:12: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:193:12: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:243:12: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:243:12: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:243:12: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:243:12: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:245:12: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:245:12: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:245:12: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:245:12: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:381:12: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:381:12: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:381:12: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:381:12: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:382:12: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:382:12: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:382:12: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:382:12: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:431:12: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:431:12: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:431:12: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:431:12: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:432:12: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:432:12: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:432:12: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:432:12: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:481:12: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:481:12: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:481:12: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:481:12: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:482:12: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:482:12: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:482:12: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:482:12: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:531:12: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:531:12: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:531:12: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:531:12: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:532:12: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:532:12: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:532:12: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:532:12: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:581:12: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:581:12: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:581:12: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:581:12: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:582:12: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:582:12: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:582:12: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:582:12: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:631:12: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:631:12: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:631:12: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:631:12: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:632:12: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:632:12: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:632:12: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:632:12: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:712:12: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:712:12: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:712:12: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:712:12: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:713:12: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:713:12: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/handlers/test_time_profilers.py:713:12: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/handlers/test_time_profilers.py:713:12: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/metrics/clustering/te

... (truncated 4243 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ty` added by @AlexWaygood on 2025-12-25 09:35_

---

_Comment by @astral-sh-bot[bot] on 2025-12-25 09:40_


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

_@AlexWaygood approved on 2025-12-27 11:40_

Thank you!!

---

_Label `breaking` added by @AlexWaygood on 2025-12-27 11:40_

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-27 11:40_

---

_Merged by @AlexWaygood on 2025-12-27 11:44_

---

_Closed by @AlexWaygood on 2025-12-27 11:44_

---

_@fhnfxr5scb-hue reviewed on 2025-12-29 19:59_

**_[]()_**

---

_Comment by @fhnfxr5scb-hue on 2025-12-29 20:01_

> **_[]()_**

- @fhnfxr5scb-hue  

---
