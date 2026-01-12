```yaml
number: 21421
title: "[ty] Support `type[…]` and `Type[…]` in implicit type aliases"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/implicit-type-aliases-subclass-of
created_at: 2025-11-13T09:45:59Z
updated_at: 2025-11-13T18:02:26Z
url: https://github.com/astral-sh/ruff/pull/21421
synced_at: 2026-01-12T15:57:23Z
```

# [ty] Support `type[…]` and `Type[…]` in implicit type aliases

---

_@sharkdp_

## Summary

Support `type[…]` in implicit type aliases, for example:
```py
SubclassOfInt = type[int]

reveal_type(SubclassOfInt)  # GenericAlias

def _(subclass_of_int: SubclassOfInt):
    reveal_type(subclass_of_int)  # type[int]
```

part of https://github.com/astral-sh/ty/issues/221

## Typing conformance

```diff
-specialtypes_type.py:138:5: error[type-assertion-failure] Argument does not have asserted type `type[Any]`
-specialtypes_type.py:140:5: error[type-assertion-failure] Argument does not have asserted type `type[Any]`
```

Two new tests passing :heavy_check_mark: 

```diff
-specialtypes_type.py:146:1: error[unresolved-attribute] Object of type `GenericAlias` has no attribute `unknown`
```

An `TA4.unknown` attribute on a PEP 613 alias (`TA4: TypeAlias = type[Any]`) is being accessed, and the conformance suite expects this to be an error. Since we currently use the inferred type for these type aliases (and possibly in the future as well), we treat this as a direct access of the attribute on `type[Any]`, which falls back to an access on `Any` itself, which succeeds. :red_circle: 

```
+specialtypes_type.py:152:16: error[invalid-type-form] `typing.TypeVar` is not a generic class
+specialtypes_type.py:156:16: error[invalid-type-form] `typing.TypeVar` is not a generic class
```

New errors because we don't handle `T = TypeVar("T"); MyType = type[T]; MyType[T]` yet. Support for this is being tracked in https://github.com/astral-sh/ty/issues/221 :red_circle:

## Ecosystem impact

Looks mostly good, a few known problems. 

## Test Plan

New Markdown tests

---

_Label `ty` added by @sharkdp on 2025-11-13 09:46_

---

_Comment by @astral-sh-bot[bot] on 2025-11-13 09:47_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-13 15:34:17.020867210 +0000
+++ new-output.txt	2025-11-13 15:34:20.477879574 +0000
@@ -900,12 +900,11 @@
 specialtypes_type.py:120:5: error[unresolved-attribute] Object of type `type` has no attribute `unknown`
 specialtypes_type.py:127:5: error[type-assertion-failure] Argument does not have asserted type `ProUser`
 specialtypes_type.py:137:5: error[type-assertion-failure] Argument does not have asserted type `type[Any]`
-specialtypes_type.py:138:5: error[type-assertion-failure] Argument does not have asserted type `type[Any]`
 specialtypes_type.py:139:5: error[type-assertion-failure] Argument does not have asserted type `type[Any]`
-specialtypes_type.py:140:5: error[type-assertion-failure] Argument does not have asserted type `type[Any]`
 specialtypes_type.py:143:1: error[unresolved-attribute] Object of type `typing.Type` has no attribute `unknown`
 specialtypes_type.py:145:1: error[unresolved-attribute] Class `type` has no attribute `unknown`
-specialtypes_type.py:146:1: error[unresolved-attribute] Object of type `GenericAlias` has no attribute `unknown`
+specialtypes_type.py:152:16: error[invalid-type-form] `typing.TypeVar` is not a generic class
+specialtypes_type.py:156:16: error[invalid-type-form] `typing.TypeVar` is not a generic class
 specialtypes_type.py:160:1: error[type-assertion-failure] Argument does not have asserted type `int`
 specialtypes_type.py:161:1: error[type-assertion-failure] Argument does not have asserted type `int`
 specialtypes_type.py:169:5: error[invalid-assignment] Object of type `type` is not assignable to `type[int]`
@@ -1001,5 +1000,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 1003 diagnostics
+Found 1002 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-13 09:47_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
more-itertools (https://github.com/more-itertools/more-itertools)
- more_itertools/more.pyi:219:16: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- more_itertools/more.pyi:220:15: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- more_itertools/more.pyi:222:23: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- more_itertools/more.pyi:642:42: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- more_itertools/more.pyi:646:52: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- Found 34 diagnostics
+ Found 29 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/_code/code.py:676:34: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/_pytest/_code/code.py:789:29: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/_pytest/_code/code.py:818:29: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- Found 474 diagnostics
+ Found 471 diagnostics

beartype (https://github.com/beartype/beartype)
- beartype/_check/error/errmain.py:399:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ beartype/_check/error/errmain.py:591:13: error[unknown-argument] Argument `message` does not match any known parameter of bound method `__init__`
+ beartype/_check/error/errmain.py:592:13: error[unknown-argument] Argument `culprits` does not match any known parameter of bound method `__init__`
+ beartype/_data/typing/datatyping.py:671:6: error[unresolved-attribute] Object of type `object` has no attribute `forward`
- beartype/_decor/decorcore.py:216:82: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ beartype/bite/_infermain.py:238:21: error[invalid-type-form] Variable of type `Never` is not allowed in a type expression
- Found 505 diagnostics
+ Found 507 diagnostics

mypy (https://github.com/python/mypy)
- mypy/typeshed/stdlib/contextlib.pyi:41:34: error[unsupported-operator] Operator `|` is unsupported between objects of type `GenericAlias` and `None`
- mypy/typeshed/stdlib/contextlib.pyi:178:6: error[unsupported-operator] Operator `|` is unsupported between objects of type `GenericAlias` and `None`
- mypy/typeshed/stdlib/inspect.pyi:282:5: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'ModuleType'>` and `GenericAlias`
- mypy/typeshed/stdlib/marshal.pyi:12:5: error[unsupported-operator] Operator `|` is unsupported between objects of type `None` and `GenericAlias`
- Found 1704 diagnostics
+ Found 1700 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/annotations.py:389:42: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- tanjun/annotations.py:414:45: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- tanjun/annotations.py:600:41: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- tanjun/annotations.py:615:41: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- tanjun/annotations.py:629:41: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- tanjun/annotations.py:2263:21: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- tanjun/annotations.py:2263:62: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ tanjun/annotations.py:2268:56: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[PartialChannel] | int`, found `object`
+ tanjun/annotations.py:2268:56: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[PartialChannel] | int`, found `object`
- tanjun/annotations.py:2276:38: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- tanjun/annotations.py:2276:72: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- tanjun/annotations.py:2289:53: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- tanjun/annotations.py:2371:50: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- Found 154 diagnostics
+ Found 145 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_core/_traps.py:67:10: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/trio/_core/_traps.py:68:16: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/trio/_core/_traps.py:75:16: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/trio/_tests/test_path.py:56:23: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/trio/_tests/test_path.py:56:39: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/trio/_tests/test_path.py:64:27: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/trio/_tests/test_path.py:64:50: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/trio/_tests/test_path.py:82:27: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/trio/_tests/test_path.py:82:42: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/trio/_tests/test_path.py:91:27: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/trio/_tests/test_path.py:91:49: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/trio/_tests/test_path.py:94:21: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_tests/test_path.py:106:12: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/trio/_tests/test_path.py:107:12: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- Found 575 diagnostics
+ Found 561 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
- django-stubs/test/utils.pyi:23:28: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- Found 475 diagnostics
+ Found 474 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/embed/standalone.py:137:49: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/bokeh/embed/standalone.py:140:12: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/bokeh/embed/standalone.py:144:49: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/bokeh/embed/standalone.py:147:12: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/bokeh/embed/standalone.py:151:49: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/bokeh/embed/standalone.py:154:12: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/bokeh/embed/standalone.py:157:52: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/bokeh/embed/standalone.py:299:22: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- src/bokeh/embed/standalone.py:370:62: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- Found 655 diagnostics
+ Found 646 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:507:49: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- pandas-stubs/_typing.pyi:527:56: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- pandas-stubs/_typing.pyi:527:64: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- pandas-stubs/_typing.pyi:927:26: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- pandas-stubs/_typing.pyi:929:26: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- pandas-stubs/_typing.pyi:930:40: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- pandas-stubs/_typing.pyi:932:28: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- pandas-stubs/_typing.pyi:934:36: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- pandas-stubs/_typing.pyi:935:26: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- pandas-stubs/_typing.pyi:948:5: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ pandas-stubs/core/series.pyi:338:57: error[invalid-argument-type] Argument to class `IndexOpsMixin` is incorrect: Expected `str | bytes | int | ... omitted 12 union elements`, found `typing.TypeVar`
- Found 5973 diagnostics
+ Found 5964 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/_typing.py:208:44: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- Found 3357 diagnostics
+ Found 3356 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/container_util.py:502:17: error[invalid-argument-type] Argument to function `to_index` is incorrect: Expected `Iterable[Hashable]`, found `(Iterable[Hashable] & ~IndexAutoFactory) | (@Todo & ~IndexAutoFactory) | None`
+ static_frame/core/container_util.py:502:17: error[invalid-argument-type] Argument to function `to_index` is incorrect: Expected `Iterable[Hashable]`, found `(Iterable[Hashable] & ~IndexAutoFactory) | type[IndexAutoFactory] | None`
- static_frame/core/container_util.py:538:77: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/index.py:995:32: error[unsupported-operator] Operator `in` is not supported for types `Any` and `type[IndexAutoFactory]`, in comparing `Any` with `(@Todo & ~(() -> object)) | (Iterable[Hashable] & ~(() -> object)) | (type[IndexAutoFactory] & ~(() -> object))`
- static_frame/core/index_hierarchy.py:1879:77: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/index_hierarchy.py:1901:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/series.py:1240:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1913 diagnostics
+ Found 1910 diagnostics

zulip (https://github.com/zulip/zulip)
- analytics/views/stats.py:349:30: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- analytics/views/stats.py:363:15: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- analytics/views/stats.py:363:34: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- analytics/views/stats.py:363:70: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- Found 3320 diagnostics
+ Found 3316 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/v1/class_validators.py:356:74: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pydantic/v1/config.py:183:12: error[invalid-return-type] Return type does not match returned value: expected `type[BaseConfig]`, found `type`
- pydantic/v1/error_wrappers.py:61:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/v1/error_wrappers.py:63:68: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pydantic/v1/main.py:1037:55: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `type[BaseModel] | type[Dataclass]`
- Found 4820 diagnostics
+ Found 4819 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-13 09:47_

---

_Comment by @astral-sh-bot[bot] on 2025-11-13 09:54_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-type-form` | 1 | 57 | 0 |
| `unused-ignore-comment` | 0 | 10 | 0 |
| `unsupported-operator` | 1 | 4 | 0 |
| `invalid-argument-type` | 3 | 0 | 1 |
| `unknown-argument` | 2 | 0 | 0 |
| `invalid-parameter-default` | 1 | 0 | 0 |
| `invalid-return-type` | 1 | 0 | 0 |
| `unresolved-attribute` | 1 | 0 | 0 |
| **Total** | **10** | **71** | **1** |

**[Full report with detailed diff](https://david-implicit-type-aliases-etn5.ecosystem-663.pages.dev/diff)** ([timing results](https://david-implicit-type-aliases-etn5.ecosystem-663.pages.dev/timing))




---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-13 15:20_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-13 15:20_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-13 15:32_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-13 15:32_

---

_Renamed from "[ty] Support `type[…]` in implicit type aliases" to "[ty] Support `type[…]` and `Type[…]` in implicit type aliases" by @sharkdp on 2025-11-13 15:43_

---

_@sharkdp reviewed on 2025-11-13 15:50_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class_base.rs`:184 on 2025-11-13 15:50_

This is mostly based on the TODO comment in `mdtest/type_of/basic.md`. Do we need to preserve more information here? I don't really understand what `class Foo(type[int]): ...` tries to achieve.

---

_Marked ready for review by @sharkdp on 2025-11-13 15:51_

---

_Review requested from @carljm by @sharkdp on 2025-11-13 15:51_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-13 15:51_

---

_Review requested from @dcreager by @sharkdp on 2025-11-13 15:51_

---

_@AlexWaygood reviewed on 2025-11-13 15:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class_base.rs`:184 on 2025-11-13 15:57_

> I don't really understand what `class Foo(type[int]): ...` tries to achieve.

Nor do I. I think this is fine.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:10632 on 2025-11-13 16:15_

this is fine for now, but note that it results in us being too permissive when parsing the argument to `type[]`. For example, on your branch:

```py
from typing import Literal

X = type[Literal[42]]  # no error here, but there should be?

def f(
    x: type[Literal[42]],  # error: [invalid-type-form]
    y: X  # no error here either...
):
    reveal_type(x)  # revealed: @Todo(unsupported nested subscript in type[X])
    reveal_type(y)  # revealed: `<class 'int'>`
```



---

_@AlexWaygood approved on 2025-11-13 16:15_

---

_@sharkdp reviewed on 2025-11-13 18:02_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:10632 on 2025-11-13 18:02_

Thanks. I'll add a test with a TODO in my next PR.

---

_Merged by @sharkdp on 2025-11-13 18:02_

---

_Closed by @sharkdp on 2025-11-13 18:02_

---

_Branch deleted on 2025-11-13 18:02_

---
