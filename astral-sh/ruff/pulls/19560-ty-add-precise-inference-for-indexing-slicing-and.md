```yaml
number: 19560
title: "[ty] Add precise inference for indexing, slicing and unpacking `NamedTuple` instances"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex-brent/namedtuples
created_at: 2025-07-25T17:27:16Z
updated_at: 2025-08-13T15:20:11Z
url: https://github.com/astral-sh/ruff/pull/19560
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Add precise inference for indexing, slicing and unpacking `NamedTuple` instances

---

_Pull request opened by @AlexWaygood on 2025-07-25 17:27_

## Summary

Infer the correct tuple supertype for `NamedTuple` classes. This allows us to infer precise types for indexing into `NamedTuple` classes, unpacking `NamedTuple` classes, and comparing `NamedTuple` classes.

```py
from typing import NamedTuple

class Foo(NamedTuple):
    x: int
    y: str

def _(f: Foo):
    reveal_type(f[0])  # revealed: int
    reveal_type(f[1])  # revealed: str
    a, b = f
    reveal_type(a)  # revealed: int
    reveal_type(b)  # revealed: str
```

## Test Plan

Mdtests.

## Typing conformance diff

This is all great! We see lots of `type-assertion-failures` going away, and lots of diagnostics now being emitted on lines in the conformance tests that are meant to produce type-checker errors.

## Ecosystem analysis

This all looks good to me! Analysis in https://github.com/astral-sh/ruff/pull/19560#issuecomment-3180074168.

## New ecosystem panics

Unfortunately this causes many new ecosystem panics, because `packaging` has a `NamedTuple` class that has a field annotated with a recursive type alias: https://github.com/pypa/packaging/blob/0055d4b8ff353455f0617690e609bc68a1f9ade2/src/packaging/_parser.py#L55. The initial support https://github.com/astral-sh/ruff/pull/19805 added for recursive types doesn't help us here, because it only adds support for explicit PEP-695 type aliases, whereas `packaging`'s type alias is an implicit one.

Many projects use `packaging`, so this unfortunately causes quite a big fallout...

---

Co-authored-by: Brent Westbrook <brentrwestbrook@gmail.com>

---

_Label `ty` added by @AlexWaygood on 2025-07-25 17:27_

---

_Comment by @github-actions[bot] on 2025-07-25 18:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
+ src/attr/_make.py:703:29: error[unresolved-attribute] Type `Attribute` has no attribute `name`
+ src/attr/_make.py:705:50: error[not-iterable] Object of type `type` is not iterable
+ src/attr/_make.py:739:22: error[not-iterable] Object of type `type` is not iterable
+ tests/test_make.py:197:52: error[not-iterable] Object of type `type` is not iterable
+ tests/test_make.py:222:25: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `type`
+ tests/test_make.py:223:54: error[not-iterable] Object of type `type` is not iterable
+ tests/test_make.py:268:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `type`
+ tests/test_make.py:271:18: error[not-iterable] Object of type `type` is not iterable
+ tests/test_make.py:287:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `type`
+ tests/test_make.py:290:18: error[not-iterable] Object of type `type` is not iterable
- Found 650 diagnostics
+ Found 660 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ pydantic/v1/types.py:704:12: error[unsupported-operator] Operator `>=` is not supported for types `str` and `int`, in comparing `int | Literal["n", "N", "F"]` with `Literal[0]`
+ pydantic/v1/types.py:706:22: error[unsupported-operator] Operator `+` is unsupported between objects of type `int` and `int | Literal["n", "N", "F"]`
+ pydantic/v1/types.py:714:20: error[invalid-argument-type] Argument to function `abs` is incorrect: Expected `SupportsAbs[Unknown]`, found `int | Literal["n", "N", "F"]`
+ pydantic/v1/types.py:715:41: error[invalid-argument-type] Argument to function `abs` is incorrect: Expected `SupportsAbs[Unknown]`, found `int | Literal["n", "N", "F"]`
+ pydantic/v1/types.py:715:41: error[invalid-argument-type] Argument to function `abs` is incorrect: Expected `SupportsAbs[Unknown]`, found `int | Literal["n", "N", "F"]`
+ pydantic/v1/types.py:718:32: error[invalid-argument-type] Argument to function `abs` is incorrect: Expected `SupportsAbs[Unknown]`, found `int | Literal["n", "N", "F"]`
- Found 757 diagnostics
+ Found 763 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- bson/decimal128.py:103:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 511 diagnostics
+ Found 510 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ cloudinit/config/modules.py:287:48: error[unresolved-attribute] Type `ModuleType` has no attribute `handle`
+ cloudinit/config/modules.py:298:39: error[unresolved-attribute] Type `ModuleType` has no attribute `handle`
- Found 614 diagnostics
+ Found 616 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
+ win32/Demos/win32gui_menu.py:354:16: error[unsupported-operator] Operator `&` is unsupported between objects of type `int | None` and `Literal[8]`
+ win32/test/test_win32guistruct.py:89:43: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
- Found 2004 diagnostics
+ Found 2006 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/pq/pq_ctypes.py:937:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 655 diagnostics
+ Found 654 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/hookspec.py:1114:6: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `TestShortLogReport | tuple[str, str, str | tuple[str, Mapping[str, bool]]]`
+ src/_pytest/hookspec.py:1114:6: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[str, str, str | tuple[str, Mapping[str, bool]]]`

sympy (https://github.com/sympy/sympy)
+ sympy/integrals/manualintegrate.py:1894:28: error[unsupported-operator] Operator `<` is not supported for types `Basic` and `int`, in comparing `Unknown | Basic` with `Literal[0]`
+ sympy/integrals/manualintegrate.py:1906:49: error[invalid-argument-type] Argument is incorrect: Expected `Expr`, found `Unknown | Basic`
- Found 12989 diagnostics
+ Found 12991 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     memo fields = ~54MB
+     memo fields = ~57MB

```
</details>


---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-07-25 18:12_

---

_Comment by @github-actions[bot] on 2025-07-25 18:22_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-overload` | 0 | 727 | 0 |
| `unresolved-import` | 0 | 267 | 0 |
| `unused-ignore-comment` | 0 | 266 | 0 |
| `invalid-argument-type` | 108 | 8 | 0 |
| `possibly-unbound-attribute` | 41 | 40 | 0 |
| `invalid-context-manager` | 0 | 56 | 0 |
| `unresolved-attribute` | 8 | 42 | 0 |
| `unsupported-operator` | 29 | 2 | 0 |
| `invalid-assignment` | 9 | 11 | 0 |
| `non-subscriptable` | 18 | 1 | 0 |
| `possibly-unresolved-reference` | 0 | 15 | 0 |
| `not-iterable` | 10 | 0 | 0 |
| `invalid-return-type` | 0 | 6 | 3 |
| `invalid-type-form` | 0 | 9 | 0 |
| `missing-argument` | 0 | 9 | 0 |
| `no-matching-overload` | 6 | 3 | 0 |
| `call-non-callable` | 1 | 4 | 0 |
| `redundant-cast` | 0 | 2 | 0 |
| `division-by-zero` | 0 | 1 | 0 |
| `unresolved-reference` | 0 | 1 | 0 |
| **Total** | **230** | **1,470** | **3** |

**[Full report with detailed diff](https://alex-brent-namedtuples.ecosystem-663.pages.dev/diff)**


---

_Comment by @AlexWaygood on 2025-07-25 18:24_

The ecosystem report indicates that we shouldn't do this until we support unpacking tuple subclasses in the same way as we support unpacking tuples.

---

_Comment by @github-actions[bot] on 2025-07-30 12:13_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-13 15:17:32.371829275 +0000
+++ new-output.txt	2025-08-13 15:17:32.438829393 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(c91d)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(14cb3)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
@@ -661,14 +661,8 @@
 literals_semantics.py:24:5: error[invalid-assignment] Object of type `Literal[0]` is not assignable to `Literal[False]`
 literals_semantics.py:25:5: error[invalid-assignment] Object of type `Literal[False]` is not assignable to `Literal[0]`
 literals_semantics.py:33:5: error[invalid-assignment] Object of type `Literal[6, 7, 8]` is not assignable to `Literal[3, 4, 5]`
-namedtuples_define_class.py:23:1: error[type-assertion-failure] Argument does not have asserted type `int`
-namedtuples_define_class.py:24:1: error[type-assertion-failure] Argument does not have asserted type `int`
-namedtuples_define_class.py:25:1: error[type-assertion-failure] Argument does not have asserted type `str`
-namedtuples_define_class.py:26:1: error[type-assertion-failure] Argument does not have asserted type `str`
-namedtuples_define_class.py:27:1: error[type-assertion-failure] Argument does not have asserted type `int`
-namedtuples_define_class.py:28:1: error[type-assertion-failure] Argument does not have asserted type `int`
-namedtuples_define_class.py:29:1: error[type-assertion-failure] Argument does not have asserted type `tuple[int, int]`
-namedtuples_define_class.py:30:1: error[type-assertion-failure] Argument does not have asserted type `tuple[int, int, str]`
+namedtuples_define_class.py:32:7: error[index-out-of-bounds] Index 3 is out of bounds for tuple `Point` with length 3
+namedtuples_define_class.py:33:7: error[index-out-of-bounds] Index -4 is out of bounds for tuple `Point` with length 3
 namedtuples_define_class.py:44:6: error[missing-argument] No argument provided for required parameter `y`
 namedtuples_define_class.py:45:6: error[missing-argument] No argument provided for required parameter `y`
 namedtuples_define_class.py:46:15: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal[""]`
@@ -679,15 +673,13 @@
 namedtuples_define_class.py:95:1: error[type-assertion-failure] Argument does not have asserted type `int | float`
 namedtuples_define_class.py:96:1: error[type-assertion-failure] Argument does not have asserted type `int | float`
 namedtuples_define_class.py:98:19: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `float`
-namedtuples_usage.py:27:1: error[type-assertion-failure] Argument does not have asserted type `int`
-namedtuples_usage.py:28:1: error[type-assertion-failure] Argument does not have asserted type `int`
-namedtuples_usage.py:29:1: error[type-assertion-failure] Argument does not have asserted type `str`
-namedtuples_usage.py:30:1: error[type-assertion-failure] Argument does not have asserted type `str`
-namedtuples_usage.py:31:1: error[type-assertion-failure] Argument does not have asserted type `int`
-namedtuples_usage.py:32:1: error[type-assertion-failure] Argument does not have asserted type `int`
+namedtuples_type_compat.py:22:1: error[invalid-assignment] Object of type `Point` is not assignable to `tuple[int, int]`
+namedtuples_type_compat.py:23:1: error[invalid-assignment] Object of type `Point` is not assignable to `tuple[int, str, str]`
+namedtuples_usage.py:34:7: error[index-out-of-bounds] Index 3 is out of bounds for tuple `Point` with length 3
+namedtuples_usage.py:35:7: error[index-out-of-bounds] Index -4 is out of bounds for tuple `Point` with length 3
 namedtuples_usage.py:41:1: error[invalid-assignment] Cannot assign to object of type `Point` with no `__setitem__` method
-namedtuples_usage.py:49:1: error[type-assertion-failure] Argument does not have asserted type `int`
-namedtuples_usage.py:50:1: error[type-assertion-failure] Argument does not have asserted type `str`
+namedtuples_usage.py:52:1: error[invalid-assignment] Too many values to unpack: Expected 2
+namedtuples_usage.py:53:1: error[invalid-assignment] Not enough values to unpack: Expected 4
 narrowing_typeguard.py:17:9: error[type-assertion-failure] Argument does not have asserted type `tuple[str, str]`
 narrowing_typeguard.py:32:9: error[type-assertion-failure] Argument does not have asserted type `set[int]`
 narrowing_typeguard.py:69:9: error[type-assertion-failure] Argument does not have asserted type `int`
@@ -868,5 +860,5 @@
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 869 diagnostics
+Found 861 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-08-05 18:20_

---

_Comment by @AlexWaygood on 2025-08-12 16:10_

# Ecosystem analysis

## `attrs`

These are all because we previously inferred the three variables [here](https://github.com/python-attrs/attrs/blob/616adf5af936b9e690e72d740440dcfc9df25c87/src/attr/_make.py#L690-L698) as all having type `Unknown`, because the `_transform_attrs` function there returns an instance of a `NamedTuple` class.

```diff
+ src/attr/_make.py:703:29: error[unresolved-attribute] Type `Attribute` has no attribute `name`
```

This is expected with our current capabilities. The [`Attribute`](https://github.com/python-attrs/attrs/blob/616adf5af936b9e690e72d740440dcfc9df25c87/src/attr/_make.py#L2413) class doesn't declare the `name` instance attribute as a type annotation, nor does it statically set the `name` instance attribute in any methods; it's only set dynamically, e.g. [here](https://github.com/python-attrs/attrs/blob/616adf5af936b9e690e72d740440dcfc9df25c87/src/attr/_make.py#L2508). There _is_ a `name` entry in the class's `__slots__`, but we don't support `__slots__` for inference of instance attributes currently.

```diff
+ src/attr/_make.py:705:50: error[not-iterable] Object of type `type` is not iterable
+ src/attr/_make.py:739:22: error[not-iterable] Object of type `type` is not iterable
+ tests/test_make.py:197:52: error[not-iterable] Object of type `type` is not iterable
+ tests/test_make.py:222:25: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `type`
+ tests/test_make.py:223:54: error[not-iterable] Object of type `type` is not iterable
+ tests/test_make.py:268:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `type`
+ tests/test_make.py:271:18: error[not-iterable] Object of type `type` is not iterable
+ tests/test_make.py:287:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `type`
+ tests/test_make.py:290:18: error[not-iterable] Object of type `type` is not iterable
```

these all look like true positives to me? We now infer the [`attrs` variable here](https://github.com/python-attrs/attrs/blob/616adf5af936b9e690e72d740440dcfc9df25c87/src/attr/_make.py#L690) as being of type `type` because it's the first element of [this `NamedTuple` class](https://github.com/python-attrs/attrs/blob/616adf5af936b9e690e72d740440dcfc9df25c87/src/attr/_make.py#L288-L291). Not totally sure what's going on here?

## Pydantic

```diff
pydantic (https://github.com/pydantic/pydantic)
+ pydantic/v1/types.py:704:12: error[unsupported-operator] Operator `>=` is not supported for types `str` and `int`, in comparing `int | Literal["n", "N", "F"]` with `Literal[0]`
+ pydantic/v1/types.py:706:22: error[unsupported-operator] Operator `+` is unsupported between objects of type `int` and `int | Literal["n", "N", "F"]`
+ pydantic/v1/types.py:714:20: error[invalid-argument-type] Argument to function `abs` is incorrect: Expected `SupportsAbs[Unknown]`, found `int | Literal["n", "N", "F"]`
+ pydantic/v1/types.py:715:41: error[invalid-argument-type] Argument to function `abs` is incorrect: Expected `SupportsAbs[Unknown]`, found `int | Literal["n", "N", "F"]`
+ pydantic/v1/types.py:715:41: error[invalid-argument-type] Argument to function `abs` is incorrect: Expected `SupportsAbs[Unknown]`, found `int | Literal["n", "N", "F"]`
+ pydantic/v1/types.py:718:32: error[invalid-argument-type] Argument to function `abs` is incorrect: Expected `SupportsAbs[Unknown]`, found `int | Literal["n", "N", "F"]`
- Found 759 diagnostics
+ Found 765 diagnostics
```

Code [here](https://github.com/pydantic/pydantic/blob/d5684659df98d55678a450690000346dd326280b/pydantic/v1/types.py#L694-L718). The relevant `NamedTuple` definition is in typeshed [here](https://github.com/python/typeshed/blob/6cae34647c8fa41060cd9de5a7de3ce6239b6b19/stdlib/decimal.pyi#L51-L54), and the relevant field annotation is `int | Literal["n", "N", "F"]`. They're trying to narrow the type to `int` by doing this:

```py
        if exponent in {'F', 'n', 'N'}:
            raise errors.DecimalIsNotFiniteError()
```

which isn't a type narrowing operation we support currently.

## `mongo-python-driver`

```diff
mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- bson/decimal128.py:103:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 508 diagnostics
+ Found 507 diagnostics
```

Code [here](https://github.com/mongodb/mongo-python-driver/blob/f105789e1245e383550731ca7cdf045003c76c47/bson/decimal128.py#L103). This is another case of code running into typeshed's annotation for `DecimalTuple.exponent`.

## cloud-init

```diff
cloud-init (https://github.com/canonical/cloud-init)
+ cloudinit/config/modules.py:287:48: error[unresolved-attribute] Type `ModuleType` has no attribute `handle`
+ cloudinit/config/modules.py:298:39: error[unresolved-attribute] Type `ModuleType` has no attribute `handle`
- Found 614 diagnostics
+ Found 616 diagnostics
```

The `NamedTuple` [here](https://github.com/canonical/cloud-init/blob/1b908e331a7baee4b1f5020800caafa6e6250707/cloudinit/config/modules.py#L52-L56) is annotated as having a `module: ModuleType` field. `ModuleType` has a `__getattr__` method in typeshed that's meant to make code like this easier to write, but we currently always ignore that `__getattr__` method when looking up attributes on `ModuleType`, due to a pre-existing bug.

## pywin32

```diff
+ win32/Demos/win32gui_menu.py:354:16: error[unsupported-operator] Operator `&` is unsupported between objects of type `int | None` and `Literal[8]`
```

This looks correct given the definition of `UnpackMENUITEMINFO` and `_MENUITEMINFO` [in typeshed's stubs for `pywin32`](https://github.com/python/typeshed/blob/6cae34647c8fa41060cd9de5a7de3ce6239b6b19/stubs/pywin32/win32/lib/win32gui_struct.pyi#L41-L52) (which are installed as part of the primer run when checking `pywin32` itself!). The pywin32 code is [here](https://github.com/mhammond/pywin32/blob/1baced97cc5f2828d00bae8c70ec5e1833b66999/win32/Demos/win32gui_menu.py#L342-L355).

```diff
+ win32/test/test_win32guistruct.py:89:43: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
```

This has the same underlying cause as the first one ([code here](https://github.com/mhammond/pywin32/blob/1baced97cc5f2828d00bae8c70ec5e1833b66999/win32/test/test_win32guistruct.py#L64-L89)).

## pyscopg, sympy

The code here is quite complex to untangle, but I can't see any obvious evidence of `NamedTuple` bugs in these either!

---

_Renamed from "[ty] Add precise inference for indexing into `NamedTuple` instances" to "[ty] Add precise inference for indexing, slicing and unpacking `NamedTuple` instances" by @AlexWaygood on 2025-08-12 17:44_

---

_Marked ready for review by @AlexWaygood on 2025-08-12 17:47_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-12 17:47_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-12 17:47_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-12 17:47_

---

_@dcreager approved on 2025-08-12 19:48_

This is great!

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:9393 on 2025-08-12 23:26_

I'm not convinced of this comment; I think the "pivot class" of a bound `super` object really is a "base class" (that is, it is anything that can exist in an MRO), and it's good to represent it with `ClassBase`.

I would frame the problem here slightly differently: `ClassBase` enum should represent "a thing that can be in an MRO", but the constructor `ClassBase::try_from_type` is also emulating the behavior of `__mro_entries__`, and there are cases for wanting to construct a "thing that can be in an MRO" from a type without emulating `__mro_entries__`. So perhaps there should be a way to construct a `ClassBase` that doesn't model `__mro_entries__` (and would thus return `None` if you tried to use `NamedTuple` as a base type.)

---

_@carljm approved on 2025-08-12 23:29_

Fantastic!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9393 on 2025-08-13 15:07_

> I would frame the problem here slightly differently: `ClassBase` enum should represent "a thing that can be in an MRO", but the constructor `ClassBase::try_from_type` is also emulating the behavior of `__mro_entries__`, and there are cases for wanting to construct a "thing that can be in an MRO" from a type without emulating `__mro_entries__`. So perhaps there should be a way to construct a `ClassBase` that doesn't model `__mro_entries__` (and would thus return `None` if you tried to use `NamedTuple` as a base type.)

Hmm, that's an interesting way of thinking about it... although we don't really attempt to _fully_ emulate `__mro_entries__` in `ClassBase::try_from_type`. We only really try to emulate the parts of `__mro_entries__` that consist of 1:1 replacements of one object for another. Some stdlib `__mro_entries__` methods implement more complex behaviour where e.g. bases are conditionally appended to the end of the bases list -- we emulate _that_ behaviour in https://github.com/astral-sh/ruff/blob/e12747a903989a14735d37e3128f15ec74ecda31/crates/ty_python_semantic/src/types/mro.rs#L73-L102

To me this sort-of goes to show even more that the methods on `ClassBase` have (in their current state, at least) been designed only with the internals of our MRO implementation in mind, and don't necessarily make sense if you try to reuse the enum for other purposes. One solution to this (the one this TODO comment currently implies) is just to create a separate enum with the same variants as `ClassBase`, which you can losslessly convert a `ClassBase` into. Since `ClassBase` is not a very big enum at all, I don't think this would lead to much code duplication.

I think `BoundSuperType` pretty clearly shouldn't be using `ClassBase::try_from_type` here, at the very least: it leads to obvious false negatives like this -- the `super()` call here fails at runtime, but we don't detect that, because `typing.ChainMap` is valid as an entry in a class's bases tuple, so the `ClassBase::try_from_type` call succeeds:

```py
import typing

# revealed: <super: <class 'ChainMap[Unknown, Unknown]'>, Unknown>
reveal_type(super(typing.ChainMap, typing.ChainMap()))
```

https://play.ty.dev/fa1b725b-8121-4250-8ad4-604301127a91

For now I'll just make the TODO comment more vague and try to clean this up in a followup.

---

_@AlexWaygood reviewed on 2025-08-13 15:07_

---

_Merged by @AlexWaygood on 2025-08-13 15:19_

---

_Closed by @AlexWaygood on 2025-08-13 15:19_

---

_Branch deleted on 2025-08-13 15:19_

---
