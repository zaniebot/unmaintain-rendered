```yaml
number: 20922
title: "[ty] Infer type for implicit `self` parameters in method bodies"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/type-of-self-in-methods-integration
created_at: 2025-10-16T14:30:35Z
updated_at: 2025-10-23T13:14:29Z
url: https://github.com/astral-sh/ruff/pull/20922
synced_at: 2026-01-12T15:57:12Z
```

# [ty] Infer type for implicit `self` parameters in method bodies

---

_@sharkdp_

## Summary

Infer a type of `Self` for unannotated `self` parameters in methods of classes.

part of https://github.com/astral-sh/ty/issues/159

closes https://github.com/astral-sh/ty/issues/1081

## Conformance tests changes

```diff
+enums_member_values.py:85:9: error[invalid-assignment] Object of type `int` is not assignable to attribute `_value_` of type `str`
```
A true positive :heavy_check_mark: 

```diff
-generics_self_advanced.py:35:9: error[type-assertion-failure] Argument does not have asserted type `Self@method2`
-generics_self_basic.py:14:9: error[type-assertion-failure] Argument does not have asserted type `Self@set_scale
```

Two false positives going away :heavy_check_mark: 

```diff
+generics_syntax_infer_variance.py:82:9: error[invalid-assignment] Cannot assign to final attribute `x` on type `Self@__init__`
```

This looks like a true positive to me, even if it's not marked with `# E` :heavy_check_mark: 

```diff
+protocols_explicit.py:56:9: error[invalid-assignment] Object of type `tuple[int, int, str]` is not assignable to attribute `rgb` of type `tuple[int, int, int]`
```

True positive :heavy_check_mark: 

```
+protocols_explicit.py:85:9: error[invalid-attribute-access] Cannot assign to ClassVar `cm1` from an instance of type `Self@__init__`
```

This looks like a true positive to me, even if it's not marked with `# E`. But this is consistent with our understanding of `ClassVar`, I think. :heavy_check_mark: 

```py
+qualifiers_final_annotation.py:52:9: error[invalid-assignment] Cannot assign to final attribute `ID4` on type `Self@__init__`
+qualifiers_final_annotation.py:65:9: error[invalid-assignment] Cannot assign to final attribute `ID7` on type `Self@method1`
```

New true positives :heavy_check_mark: 

```py
+qualifiers_final_annotation.py:52:9: error[invalid-assignment] Cannot assign to final attribute `ID4` on type `Self@__init__`
+qualifiers_final_annotation.py:57:13: error[invalid-assignment] Cannot assign to final attribute `ID6` on type `Self@__init__`
+qualifiers_final_annotation.py:59:13: error[invalid-assignment] Cannot assign to final attribute `ID6` on type `Self@__init__`
```

This is a new false positive, but that's a pre-existing issue on main (if you annotate with `Self`): https://play.ty.dev/3ee1c56d-7e13-43bb-811a-7a81e236e6ab  :x: => reported as https://github.com/astral-sh/ty/issues/1409

## Ecosystem

* There are 5931 new `unresolved-attribute` and 3292 new `possibly-missing-attribute` attribute errors, way too many to look at all of them. I randomly sampled 15 of these errors and found: 
  * 13 instances where there was simply no such attribute that we could plausibly see. Sometimes [I didn't find it anywhere](https://github.com/internetarchive/openlibrary/blob/8644d886c6579a5f49faadc4cd1ba9992e603d7e/openlibrary/plugins/openlibrary/tests/test_listapi.py#L33). Sometimes it was set externally on the object. Sometimes there was some [`setattr` dynamicness going on](https://github.com/pypa/setuptools/blob/a49f6b927d83b97630b4fb030de8035ed32436fd/setuptools/wheel.py#L88-L94). I would consider all of them to be true positives.
  * 1 instance where [attribute was set on `obj` in `__new__`](https://github.com/sympy/sympy/blob/9e87b44fd43572b9c4cc95ec569a2f4b81d56499/sympy/tensor/array/array_comprehension.py#L45C1-L45C36), which we don't support yet
  * 1 instance [where the attribute was defined via `__slots__`
](https://github.com/spack/spack/blob/e250ec0fc81130b708a8abe1894f0cc926880210/lib/spack/spack/vendor/pyrsistent/_pdeque.py#L48C5-L48C14)
* I see 44 instances [of the false positive above](https://github.com/astral-sh/ty/issues/1409) with `Final` instance attributes being set in `__init__`. I don't think this should block this PR.

## Test Plan

New Markdown tests.

---

_Label `ty` added by @sharkdp on 2025-10-16 14:30_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-16 14:40_

---

_Comment by @github-actions[bot] on 2025-10-16 14:41_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-23 07:17:38.427123571 +0000
+++ new-output.txt	2025-10-23 07:17:41.924130267 +0000
@@ -1,5 +1,5 @@
 fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/execute.rs:402:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(d817)): execute: too many cycle iterations`
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/execute.rs:402:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(17c43)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/execute.rs:402:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(18043)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
@@ -352,6 +352,7 @@
 enums_member_names.py:30:5: error[type-assertion-failure] Argument does not have asserted type `Literal["RED", "BLUE", "GREEN"]`
 enums_member_values.py:30:5: error[type-assertion-failure] Argument does not have asserted type `Literal[1, 2, 3]`
 enums_member_values.py:54:1: error[type-assertion-failure] Argument does not have asserted type `Literal[1]`
+enums_member_values.py:85:9: error[invalid-assignment] Object of type `int` is not assignable to attribute `_value_` of type `str`
 enums_member_values.py:96:1: error[type-assertion-failure] Argument does not have asserted type `int`
 enums_members.py:128:21: info[revealed-type] Revealed type: `Unknown | Literal[2]`
 enums_members.py:129:9: error[type-assertion-failure] Argument does not have asserted type `Unknown`
@@ -445,7 +446,6 @@
 generics_self_advanced.py:18:1: error[type-assertion-failure] Argument does not have asserted type `ParentA`
 generics_self_advanced.py:19:1: error[type-assertion-failure] Argument does not have asserted type `ChildA`
 generics_self_advanced.py:28:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@method1`
-generics_self_advanced.py:35:9: error[type-assertion-failure] Argument does not have asserted type `Self@method2`
 generics_self_advanced.py:36:9: error[type-assertion-failure] Argument does not have asserted type `list[Self@method2]`
 generics_self_advanced.py:37:9: error[type-assertion-failure] Argument does not have asserted type `Self@method2`
 generics_self_advanced.py:38:9: error[type-assertion-failure] Argument does not have asserted type `Self@method2`
@@ -455,7 +455,6 @@
 generics_self_attributes.py:26:33: error[invalid-argument-type] Argument is incorrect: Expected `typing.Self | None`, found `LinkedList[int]`
 generics_self_attributes.py:29:5: error[invalid-assignment] Object of type `OrdinalLinkedList` is not assignable to attribute `next` of type `typing.Self | None`
 generics_self_attributes.py:32:5: error[invalid-assignment] Object of type `LinkedList[int]` is not assignable to attribute `next` of type `typing.Self | None`
-generics_self_basic.py:14:9: error[type-assertion-failure] Argument does not have asserted type `Self@set_scale`
 generics_self_basic.py:20:16: error[invalid-return-type] Return type does not match returned value: expected `Self@method2`, found `Shape`
 generics_self_basic.py:33:16: error[invalid-return-type] Return type does not match returned value: expected `Self@cls_method2`, found `Shape`
 generics_self_basic.py:54:1: error[type-assertion-failure] Argument does not have asserted type `Shape`
@@ -499,6 +498,7 @@
 generics_syntax_infer_variance.py:46:1: error[invalid-assignment] Object of type `ShouldBeCovariant3[int | float]` is not assignable to `ShouldBeCovariant3[int]`
 generics_syntax_infer_variance.py:74:1: error[invalid-assignment] Object of type `ShouldBeCovariant5[int]` is not assignable to `ShouldBeCovariant5[int | float]`
 generics_syntax_infer_variance.py:75:1: error[invalid-assignment] Object of type `ShouldBeCovariant5[int | float]` is not assignable to `ShouldBeCovariant5[int]`
+generics_syntax_infer_variance.py:82:9: error[invalid-assignment] Cannot assign to final attribute `x` on type `Self@__init__`
 generics_syntax_infer_variance.py:85:1: error[invalid-assignment] Object of type `ShouldBeCovariant6[int]` is not assignable to `ShouldBeCovariant6[int | float]`
 generics_syntax_infer_variance.py:86:1: error[invalid-assignment] Object of type `ShouldBeCovariant6[int | float]` is not assignable to `ShouldBeCovariant6[int]`
 generics_syntax_infer_variance.py:102:1: error[invalid-assignment] Object of type `ShouldBeInvariant1[int]` is not assignable to `ShouldBeInvariant1[int | float]`
@@ -764,6 +764,8 @@
 protocols_definition.py:287:1: error[invalid-assignment] Object of type `Concrete5_Bad3` is not assignable to `Template5`
 protocols_definition.py:288:1: error[invalid-assignment] Object of type `Concrete5_Bad4` is not assignable to `Template5`
 protocols_definition.py:289:1: error[invalid-assignment] Object of type `Concrete5_Bad5` is not assignable to `Template5`
+protocols_explicit.py:56:9: error[invalid-assignment] Object of type `tuple[int, int, str]` is not assignable to attribute `rgb` of type `tuple[int, int, int]`
+protocols_explicit.py:85:9: error[invalid-attribute-access] Cannot assign to ClassVar `cm1` from an instance of type `Self@__init__`
 protocols_generic.py:40:1: error[invalid-assignment] Object of type `Concrete1` is not assignable to `Proto1[int, str]`
 protocols_generic.py:56:5: error[invalid-assignment] Object of type `Box[int | float]` is not assignable to `Box[int]`
 protocols_generic.py:66:5: error[invalid-assignment] Object of type `Sender[int]` is not assignable to `Sender[int | float]`
@@ -808,6 +810,11 @@
 qualifiers_annotated.py:64:8: error[invalid-type-form] Special form `typing.Annotated` expected at least 2 arguments (one type and at least one metadata element)
 qualifiers_annotated.py:91:1: error[call-non-callable] Object of type `typing.Annotated` is not callable
 qualifiers_final_annotation.py:18:7: error[invalid-type-form] Type qualifier `typing.Final` expected exactly 1 argument, got 2
+qualifiers_final_annotation.py:52:9: error[invalid-assignment] Cannot assign to final attribute `ID4` on type `Self@__init__`
+qualifiers_final_annotation.py:54:9: error[invalid-assignment] Cannot assign to final attribute `ID5` on type `Self@__init__`
+qualifiers_final_annotation.py:57:13: error[invalid-assignment] Cannot assign to final attribute `ID6` on type `Self@__init__`
+qualifiers_final_annotation.py:59:13: error[invalid-assignment] Cannot assign to final attribute `ID6` on type `Self@__init__`
+qualifiers_final_annotation.py:65:9: error[invalid-assignment] Cannot assign to final attribute `ID7` on type `Self@method1`
 qualifiers_final_annotation.py:71:1: error[invalid-assignment] Reassignment of `Final` symbol `RATE` is not allowed: Symbol later reassigned here
 qualifiers_final_annotation.py:81:1: error[invalid-assignment] Cannot assign to final attribute `DEFAULT_ID` on type `<class 'ClassB'>`
 qualifiers_final_annotation.py:118:9: error[invalid-type-form] Type qualifier `typing.Final` is not allowed in type expressions (only in annotation expressions)
@@ -915,5 +922,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 917 diagnostics
+Found 924 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_@sharkdp reviewed on 2025-10-16 14:43_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/attributes.md`:1849 on 2025-10-16 14:43_

Probably something we should look at or create an issue for.

---

_Comment by @github-actions[bot] on 2025-10-16 14:46_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 5,931 | 0 | 0 |
| `possibly-missing-attribute` | 3,292 | 8 | 21 |
| `invalid-argument-type` | 1,989 | 6 | 117 |
| `unused-ignore-comment` | 0 | 1,115 | 0 |
| `unsupported-operator` | 939 | 2 | 5 |
| `invalid-assignment` | 496 | 8 | 15 |
| `invalid-return-type` | 386 | 74 | 14 |
| `non-subscriptable` | 413 | 0 | 0 |
| `call-non-callable` | 220 | 0 | 0 |
| `too-many-positional-arguments` | 174 | 0 | 0 |
| `not-iterable` | 130 | 0 | 0 |
| `no-matching-overload` | 120 | 0 | 0 |
| `missing-argument` | 114 | 0 | 0 |
| `possibly-missing-implicit-call` | 92 | 4 | 10 |
| `unknown-argument` | 66 | 0 | 0 |
| `deprecated` | 28 | 0 | 0 |
| `parameter-already-assigned` | 17 | 4 | 0 |
| `possibly-unresolved-reference` | 0 | 19 | 0 |
| `redundant-cast` | 18 | 0 | 0 |
| `division-by-zero` | 6 | 4 | 0 |
| `index-out-of-bounds` | 6 | 0 | 0 |
| `invalid-await` | 4 | 0 | 1 |
| `invalid-context-manager` | 3 | 0 | 0 |
| `type-assertion-failure` | 0 | 3 | 0 |
| `invalid-attribute-access` | 2 | 0 | 0 |
| `invalid-key` | 2 | 0 | 0 |
| `missing-typed-dict-key` | 2 | 0 | 0 |
| `unsupported-base` | 2 | 0 | 0 |
| `invalid-parameter-default` | 1 | 0 | 0 |
| `invalid-super-argument` | 1 | 0 | 0 |
| `invalid-type-form` | 1 | 0 | 0 |
| **Total** | **14,455** | **1,247** | **183** |

**[Full report with detailed diff](https://david-type-of-self-in-method-yb0u.ecosystem-663.pages.dev/diff)** ([timing results](https://david-type-of-self-in-method-yb0u.ecosystem-663.pages.dev/timing))


---

_Comment by @github-actions[bot] on 2025-10-16 14:55_

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

_Comment by @codspeed-hq[bot] on 2025-10-16 14:55_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Ftype-of-self-in-methods-integration?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #20922 will **degrade performances by 16.56%**

<sub>Comparing <code>david/type-of-self-in-methods-integration</code> (0c3d16a) with <code>main</code> (6c18f18)</sub>



### Summary

`❌ 7 (👁 7)` regressions  
`✅ 44` untouched  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| 👁 | Simulation | [`` anyio ``](https://codspeed.io/astral-sh/ruff/branches/david%2Ftype-of-self-in-methods-integration?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Aanyio%3A%3Aproject%3A%3Aanyio&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 825.7 ms | 926.1 ms | -10.84% |
| 👁 | WallTime | [`` large[sympy] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Ftype-of-self-in-methods-integration?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bsympy%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 39.5 s | 42.9 s | -7.89% |
| 👁 | WallTime | [`` medium[pandas] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Ftype-of-self-in-methods-integration?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bpandas%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 33.9 s | 35.5 s | -4.35% |
| 👁 | WallTime | [`` medium[static-frame] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Ftype-of-self-in-methods-integration?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bstatic-frame%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 9.1 s | 10.1 s | -9.09% |
| 👁 | WallTime | [`` small[freqtrade] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Ftype-of-self-in-methods-integration?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Bfreqtrade%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 4.3 s | 4.9 s | -12.96% |
| 👁 | WallTime | [`` small[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Ftype-of-self-in-methods-integration?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Bpydantic%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 2.4 s | 2.6 s | -7.41% |
| 👁 | WallTime | [`` small[tanjun] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Ftype-of-self-in-methods-integration?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Btanjun%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.6 s | 1.9 s | -16.56% |


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-22 12:31_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-22 12:31_

---

_Comment by @github-actions[bot] on 2025-10-22 12:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
zipp (https://github.com/jaraco/zipp)
+ zipp/__init__.py:94:16: error[unresolved-attribute] Object of type `Self@__getstate__` has no attribute `_saved___init__`
+ zipp/__init__.py:94:43: error[unresolved-attribute] Object of type `Self@__getstate__` has no attribute `_saved___init__`
+ zipp/__init__.py:436:16: error[no-matching-overload] No overload of bound method `format` matches arguments
- Found 1 diagnostic
+ Found 4 diagnostics

pyp (https://github.com/hauntsaninja/pyp)
- pyp.py:468:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 6 diagnostics
+ Found 5 diagnostics

bidict (https://github.com/jab/bidict)
+ bidict/_base.py:468:48: error[parameter-already-assigned] Multiple values provided for parameter `unwrites` of bound method `_write`
+ bidict/_orderedbase.py:71:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Self@__init__` with custom `__set__` method
+ bidict/_orderedbase.py:77:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Node` with custom `__set__` method
+ bidict/_orderedbase.py:78:9: error[invalid-assignment] Object of type `Node` is not assignable to attribute `prv` on type `Node | WeakAttr[Node]`
+ bidict/_orderedbase.py:82:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Node` with custom `__set__` method
+ bidict/_orderedbase.py:82:24: error[invalid-assignment] Object of type `Self@relink` is not assignable to attribute `prv` on type `Node | WeakAttr[Node]`
+ bidict/_orderedbase.py:110:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Node` with custom `__set__` method
+ bidict/_orderedbidict.py:80:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Node` with custom `__set__` method
+ bidict/_orderedbidict.py:81:9: error[invalid-assignment] Object of type `Node` is not assignable to attribute `prv` on type `Node | WeakAttr[Node]`
+ bidict/_orderedbidict.py:86:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Node` with custom `__set__` method
+ bidict/_orderedbidict.py:87:24: error[invalid-assignment] Object of type `Node` is not assignable to attribute `nxt` on type `Unknown | Node`
+ bidict/_orderedbidict.py:91:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Node` with custom `__set__` method
- Found 13 diagnostics
+ Found 25 diagnostics

pegen (https://github.com/we-like-parsers/pegen)
- src/pegen/grammar_parser.py:160:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str] | None`, found `tuple[Unknown, None]`
+ src/pegen/grammar_parser.py:160:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str] | None`, found `tuple[str, None]`
+ src/pegen/grammar_parser.py:438:31: error[no-matching-overload] No overload of bound method `join` matches arguments
+ src/pegen/python_generator.py:270:51: error[unresolved-attribute] Object of type `GrammarVisitor` has no attribute `keywords`
+ src/pegen/python_generator.py:271:56: error[unresolved-attribute] Object of type `GrammarVisitor` has no attribute `soft_keywords`
- Found 47 diagnostics
+ Found 50 diagnostics

attrs (https://github.com/python-attrs/attrs)
+ src/attr/_make.py:797:13: error[invalid-assignment] Object of type `ReferenceType[Unknown]` is not assignable to attribute `__attrs_base_of_slotted__` on type `Unknown | type`
+ src/attr/_make.py:842:13: error[invalid-assignment] Object of type `Literal[False]` is not assignable to attribute `__attrs_own_setattr__` on type `Unknown | type`
+ src/attr/_make.py:1061:13: error[invalid-argument-type] Argument to function `_make_hash_script` is incorrect: Expected `list[Attribute | Unknown]`, found `Unknown | type`
+ src/attr/_make.py:1107:26: error[not-iterable] Object of type `Unknown | type` may not be iterable
+ src/attr/_make.py:1139:41: error[invalid-argument-type] Argument to function `_make_eq_script` is incorrect: Expected `list[Unknown]`, found `Unknown | type`
+ src/attr/_make.py:1162:18: error[not-iterable] Object of type `Unknown | type` may not be iterable
+ src/attr/_make.py:2598:65: error[unresolved-attribute] Object of type `Self@__getstate__` has no attribute `metadata`
+ tests/test_cmp.py:321:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `Unknown | str | None`
+ tests/test_cmp.py:329:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `Unknown | str | None`
+ tests/test_cmp.py:389:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `Unknown | str | None`
+ tests/test_cmp.py:397:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `Unknown | str | None`
+ tests/test_cmp.py:407:18: warning[possibly-missing-attribute] Attribute `__lt__` may be missing on object of type `Unknown | type`
+ tests/test_cmp.py:415:18: warning[possibly-missing-attribute] Attribute `__le__` may be missing on object of type `Unknown | type`
+ tests/test_cmp.py:425:18: warning[possibly-missing-attribute] Attribute `__gt__` may be missing on object of type `Unknown | type`
+ tests/test_cmp.py:435:18: warning[possibly-missing-attribute] Attribute `__ge__` may be missing on object of type `Unknown | type`
+ tests/test_cmp.py:461:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `Unknown | str | None`
+ tests/test_cmp.py:469:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `Unknown | str | None`
+ tests/test_cmp.py:479:18: warning[possibly-missing-attribute] Attribute `__lt__` may be missing on object of type `Unknown | type`
+ tests/test_cmp.py:487:18: warning[possibly-missing-attribute] Attribute `__le__` may be missing on object of type `Unknown | type`
+ tests/test_cmp.py:495:18: warning[possibly-missing-attribute] Attribute `__gt__` may be missing on object of type `Unknown | type`
+ tests/test_cmp.py:503:18: warning[possibly-missing-attribute] Attribute `__ge__` may be missing on object of type `Unknown | type`
+ tests/test_make.py:2174:24: warning[possibly-missing-attribute] Attribute `__qualname__` may be missing on object of type `Unknown | ((...) -> Unknown)`
+ tests/test_slots.py:985:24: error[unresolved-attribute] Object of type `<super: <class 'B'>, B>` has no attribute `__getattr__`
- Found 561 diagnostics
+ Found 584 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
+ aioredis/connection.py:206:20: error[invalid-return-type] Return type does not match returned value: expected `ResponseError`, found `Exception | @Todo`
- aioredis/connection.py:352:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- aioredis/connection.py:803:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ aioredis/lock.py:251:19: error[call-non-callable] Object of type `None` is not callable
+ aioredis/lock.py:279:19: error[call-non-callable] Object of type `None` is not callable
+ aioredis/lock.py:299:23: error[unsupported-operator] Operator `*` is unsupported between objects of type `Unknown | int | float | None` and `Literal[1000]`
+ aioredis/lock.py:301:19: error[call-non-callable] Object of type `None` is not callable
- Found 14 diagnostics
+ Found 17 diagnostics

packaging (https://github.com/pypa/packaging)
- src/packaging/metadata.py:557:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/requirements.py:47:13: error[invalid-assignment] Object of type `Any` is not assignable to attribute `_markers` on type `Marker | None`
- Found 23 diagnostics
+ Found 21 diagnostics

anyio (https://github.com/agronholm/anyio)
+ src/anyio/_backends/_asyncio.py:166:35: error[invalid-argument-type] Argument to function `_cancel_all_tasks` is incorrect: Expected `AbstractEventLoop`, found `AbstractEventLoop | None`
+ src/anyio/_backends/_asyncio.py:167:17: warning[possibly-missing-attribute] Attribute `run_until_complete` may be missing on object of type `AbstractEventLoop | None`
+ src/anyio/_backends/_asyncio.py:167:41: warning[possibly-missing-attribute] Attribute `shutdown_asyncgens` may be missing on object of type `AbstractEventLoop | None`
+ src/anyio/_backends/_asyncio.py:171:21: error[unresolved-attribute] Object of type `None` has no attribute `run_until_complete`
+ src/anyio/_backends/_asyncio.py:171:72: error[invalid-argument-type] Argument to function `_shutdown_default_executor` is incorrect: Expected `AbstractEventLoop`, found `None`
+ src/anyio/_backends/_asyncio.py:175:17: warning[possibly-missing-attribute] Attribute `close` may be missing on object of type `AbstractEventLoop | None`
+ src/anyio/_backends/_asyncio.py:182:20: error[invalid-return-type] Return type does not match returned value: expected `AbstractEventLoop`, found `AbstractEventLoop | None`
+ src/anyio/_backends/_asyncio.py:256:17: warning[possibly-missing-attribute] Attribute `call_soon_threadsafe` may be missing on object of type `AbstractEventLoop | None`
+ src/anyio/_backends/_asyncio.py:495:21: error[unresolved-attribute] Object of type `Task[Unknown]` has no attribute `uncancel`
- src/anyio/_backends/_asyncio.py:569:36: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_backends/_asyncio.py:574:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_backends/_asyncio.py:1059:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_backends/_asyncio.py:2153:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_backends/_asyncio.py:2156:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_backends/_asyncio.py:2157:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/_core/_fileio.py:407:51: error[unknown-argument] Argument `case_sensitive` does not match any known parameter of bound method `match`
+ src/anyio/_core/_fileio.py:617:57: error[unknown-argument] Argument `walk_up` does not match any known parameter of bound method `relative_to`
+ src/anyio/_core/_tempfile.py:330:15: error[no-matching-overload] No overload of bound method `write` matches arguments
+ src/anyio/streams/stapled.py:121:34: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[Listener[T_Stream@MultiListener]]`, found `Sequence[Listener[object]]`
- Found 222 diagnostics
+ Found 229 diagnostics

parso (https://github.com/davidhalter/parso)
+ parso/normalizer.py:56:13: error[unresolved-attribute] Object of type `type` has no attribute `feed_node`
+ parso/normalizer.py:62:13: error[unresolved-attribute] Object of type `type` has no attribute `feed_node`
+ parso/pgen2/generator.py:100:9: error[invalid-assignment] Cannot assign to object of type `Mapping[str, DFAState[Unknown]]` with no `__setitem__` method
+ parso/pgen2/generator.py:105:17: error[invalid-assignment] Cannot assign to object of type `Mapping[str, DFAState[Unknown]]` with no `__setitem__` method
+ parso/python/pep8.py:120:13: error[unsupported-operator] Operator `+=` is unsupported between objects of type `None` and `Literal[" "]`
+ parso/python/pep8.py:290:22: warning[possibly-missing-attribute] Attribute `get_latest_suite_node` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:362:17: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:363:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:385:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:414:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:415:20: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:418:35: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:419:54: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:431:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:432:25: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None`
+ parso/python/pep8.py:433:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:436:41: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:441:51: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:444:49: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:449:29: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:460:49: warning[possibly-missing-attribute] Attribute `bracket_indentation` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:462:49: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:464:29: warning[possibly-missing-attribute] Attribute `get_latest_suite_node` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:471:36: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:486:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:492:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:498:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:507:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:513:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:537:24: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:538:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:541:27: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:631:33: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `None | Unknown`
+ parso/python/pep8.py:651:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `None | Unknown`
+ parso/python/pep8.py:656:41: warning[possibly-missing-attribute] Attribute `prefix` may be missing on object of type `Unknown | None`
+ parso/python/pep8.py:657:41: warning[possibly-missing-attribute] Attribute `prefix` may be missing on object of type `Unknown | None`
+ parso/python/pep8.py:679:21: error[unresolved-attribute] Object of type `Self@_analyse_non_prefix` has no attribute `add_issuadd_issue`
+ parso/python/pep8.py:767:16: error[unresolved-attribute] Object of type `Self@is_issue` has no attribute `_newline_count`
+ parso/python/tree.py:78:12: error[unresolved-attribute] Object of type `Self@get_doc_node` has no attribute `type`
+ parso/python/tree.py:79:20: error[unresolved-attribute] Object of type `Self@get_doc_node` has no attribute `children`
+ parso/python/tree.py:80:14: error[unresolved-attribute] Object of type `Self@get_doc_node` has no attribute `type`
+ parso/python/tree.py:81:20: error[unresolved-attribute] Object of type `Self@get_doc_node` has no attribute `children`
+ parso/python/tree.py:81:34: error[unresolved-attribute] Object of type `Self@get_doc_node` has no attribute `children`
+ parso/python/tree.py:85:27: error[unresolved-attribute] Object of type `Self@get_doc_node` has no attribute `parent`
+ parso/python/tree.py:110:18: error[unresolved-attribute] Object of type `Self@get_name_of_position` has no attribute `children`
+ parso/python/tree.py:219:17: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `BaseNode | None`
+ parso/python/tree.py:222:24: error[unresolved-attribute] Object of type `BaseNode | None` has no attribute `name`
+ parso/python/tree.py:228:24: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `BaseNode | None`
+ parso/python/tree.py:235:28: error[unresolved-attribute] Object of type `BaseNode` has no attribute `get_defined_names`
+ parso/python/tree.py:306:20: error[unresolved-attribute] Object of type `Self@__eq__` has no attribute `value`
+ parso/python/tree.py:311:21: error[unresolved-attribute] Object of type `Self@__hash__` has no attribute `value`
+ parso/python/tree.py:371:20: error[unresolved-attribute] Object of type `Self@__repr__` has no attribute `name`
+ parso/python/tree.py:453:12: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `BaseNode | None`
+ parso/python/tree.py:454:25: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `BaseNode | None`
+ parso/python/tree.py:456:12: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `BaseNode | None`
+ parso/python/tree.py:457:16: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `BaseNode | None`
+ parso/python/tree.py:458:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `BaseNode | None`
+ parso/python/tree.py:458:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:460:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `BaseNode | None`
+ parso/python/tree.py:554:31: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:558:13: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:561:16: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:767:23: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:785:41: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:806:20: error[unresolved-attribute] Object of type `Self@get_path_for_name` has no attribute `_aliases`
+ parso/python/tree.py:810:21: error[unresolved-attribute] Object of type `Self@get_path_for_name` has no attribute `get_paths`
+ parso/python/tree.py:844:20: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:869:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:876:23: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:917:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:923:25: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:924:27: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:931:23: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:1049:23: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:1057:20: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:1058:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:1060:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:1069:20: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:1072:21: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/tree.py:63:28: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `BaseNode | None`
+ parso/tree.py:82:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `BaseNode | None`
+ parso/tree.py:94:17: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `BaseNode | None`
+ parso/tree.py:98:20: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `BaseNode | None`
+ parso/tree.py:106:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/tree.py:120:17: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `BaseNode | None`
+ parso/tree.py:124:20: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `BaseNode | None`
+ parso/tree.py:132:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- Found 75 diagnostics
+ Found 162 diagnostics

python-chess (https://github.com/niklasf/python-chess)
+ chess/__init__.py:1558:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `BaseBoard`
+ chess/__init__.py:1842:20: error[invalid-return-type] Return type does not match returned value: expected `Self@root`, found `Board`
+ chess/__init__.py:3951:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `Board`
- chess/engine.py:878:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- chess/engine.py:905:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- chess/engine.py:908:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- chess/pgn.py:353:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ chess/engine.py:506:20: error[unsupported-operator] Operator `<` is not supported for types `int` and `None`, in comparing `tuple[bool, bool, bool, int, int | None]` with `tuple[bool, bool, bool, int, int | None]`
+ chess/engine.py:512:20: error[unsupported-operator] Operator `<=` is not supported for types `int` and `None`, in comparing `tuple[bool, bool, bool, int, int | None]` with `tuple[bool, bool, bool, int, int | None]`
+ chess/engine.py:518:20: error[unsupported-operator] Operator `>` is not supported for types `int` and `None`, in comparing `tuple[bool, bool, bool, int, int | None]` with `tuple[bool, bool, bool, int, int | None]`
+ chess/engine.py:524:20: error[unsupported-operator] Operator `>=` is not supported for types `int` and `None`, in comparing `tuple[bool, bool, bool, int, int | None]` with `tuple[bool, bool, bool, int, int | None]`
+ chess/pgn.py:1050:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `Headers`
+ chess/variant.py:808:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `ThreeCheckBoard`
+ chess/variant.py:814:20: error[invalid-return-type] Return type does not match returned value: expected `Self@root`, found `ThreeCheckBoard`
+ chess/variant.py:822:16: error[invalid-return-type] Return type does not match returned value: expected `Self@mirror`, found `ThreeCheckBoard`
+ chess/variant.py:876:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `CrazyhousePocket`
+ chess/variant.py:1061:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `CrazyhouseBoard`
+ chess/variant.py:1067:20: error[invalid-return-type] Return type does not match returned value: expected `Self@root`, found `CrazyhouseBoard`
+ chess/variant.py:1075:16: error[invalid-return-type] Return type does not match returned value: expected `Self@mirror`, found `CrazyhouseBoard`
- Found 12 diagnostics
+ Found 23 diagnostics

nionutils (https://github.com/nion-software/nionutils)
+ nion/utils/ListModel.py:632:59: error[unresolved-attribute] Object of type `object` has no attribute `listen`
+ nion/utils/ListModel.py:633:57: error[unresolved-attribute] Object of type `object` has no attribute `listen`
- Found 8 diagnostics
+ Found 10 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- pyinstrument/magic/_utils.py:73:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/magic/_utils.py:74:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pyinstrument/session.py:213:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, str].__setitem__(key: str, value: str, /) -> None` cannot be called with a key of type `str` and a value of type `str | bytes` on object of type `dict[str, str]`
+ pyinstrument/vendor/decorator.py:165:31: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `Unknown | None`

kornia (https://github.com/kornia/kornia)
- kornia/augmentation/container/augment.py:343:99: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/augment.py:405:35: error[unresolved-attribute] Object of type `object` has no attribute `type`
+ kornia/augmentation/container/augment.py:414:35: error[unresolved-attribute] Object of type `object` has no attribute `type`
- kornia/augmentation/container/augment.py:473:99: warning[unused-ignore-comment] Unused blanket `type: ignore` directive

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ tests/conftest.py:298:54: error[invalid-argument-type] Argument to bound method `run_pytest` is incorrect: Expected `Literal[False]`, found `bool`
- Found 170 diagnostics
+ Found 171 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/binary_distribution.py:432:31: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ lib/spack/spack/binary_distribution.py:432:31: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ lib/spack/spack/binary_distribution.py:437:24: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ lib/spack/spack/binary_distribution.py:437:24: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ lib/spack/spack/binary_distribution.py:445:25: error[invalid-assignment] Method `__setitem__` of type `bound method dict[MirrorURLAndVersion, int | float].__setitem__(key: MirrorURLAndVersion, value: int | float, /) -> None` cannot be called with a key of type `Unknown` and a value of type `tuple[int | float, Literal[True]]` on object of type `dict[MirrorURLAndVersion, int | float]`
+ lib/spack/spack/binary_distribution.py:450:25: error[invalid-assignment] Method `__setitem__` of type `bound method dict[MirrorURLAndVersion, int | float].__setitem__(key: MirrorURLAndVersion, value: int | float, /) -> None` cannot be called with a key of type `Unknown` and a value of type `tuple[int | float, Literal[False]]` on object of type `dict[MirrorURLAndVersion, int | float]`
+ lib/spack/spack/binary_distribution.py:484:17: error[invalid-assignment] Method `__setitem__` of type `bound method dict[MirrorURLAndVersion, int | float].__setitem__(key: MirrorURLAndVersion, value: int | float, /) -> None` cannot be called with a key of type `@Todo` and a value of type `tuple[int | float, Literal[True]]` on object of type `dict[MirrorURLAndVersion, int | float]`
+ lib/spack/spack/binary_distribution.py:489:17: error[invalid-assignment] Method `__setitem__` of type `bound method dict[MirrorURLAndVersion, int | float].__setitem__(key: MirrorURLAndVersion, value: int | float, /) -> None` cannot be called with a key of type `@Todo` and a value of type `tuple[int | float, Literal[False]]` on object of type `dict[MirrorURLAndVersion, int | float]`
- lib/spack/spack/builder.py:471:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/builder.py:497:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/ci/common.py:196:23: error[invalid-argument-type] Argument to function `win_quote` is incorrect: Expected `str`, found `str | None`
+ lib/spack/spack/config.py:566:9: error[invalid-assignment] Object of type `ConfigScope` is not assignable to `str`
+ lib/spack/spack/config.py:567:16: error[unresolved-attribute] Object of type `str` has no attribute `get_section_filename`
+ lib/spack/spack/environment/environment.py:1271:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, dict[str, list[str]]].__setitem__(key: str, value: dict[str, list[str]], /) -> None` cannot be called with a key of type `str` and a value of type `dict[Unknown | str, Unknown | list[Unknown] | dict[Unknown, Unknown]]` on object of type `dict[str, dict[str, list[str]]]`
+ lib/spack/spack/environment/environment.py:1282:21: error[unresolved-attribute] Object of type `list[str]` has no attribute `update`
+ lib/spack/spack/environment/environment.py:1290:17: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, list[str]].__setitem__(key: str, value: list[str], /) -> None` cannot be called with a key of type `Literal["include_concrete"]` and a value of type `dict[str, dict[str, list[str]]] & ~AlwaysFalsy` on object of type `dict[str, list[str]]`
+ lib/spack/spack/fetch_strategy.py:312:34: warning[division-by-zero] Cannot divide object of type `Literal[0]` by zero
+ lib/spack/spack/fetch_strategy.py:688:20: error[no-matching-overload] No overload of bound method `get` matches arguments
+ lib/spack/spack/fetch_strategy.py:952:16: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1039:16: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1085:48: error[not-iterable] Object of type `(Unknown & ~(() -> object)) | None` may not be iterable
+ lib/spack/spack/fetch_strategy.py:1107:16: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1259:26: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1348:26: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1428:16: error[unresolved-attribute] Object of type `Self@source_id` has no attribute `revision`
+ lib/spack/spack/fetch_strategy.py:1431:12: error[unresolved-attribute] Object of type `Self@mirror_id` has no attribute `revision`
+ lib/spack/spack/fetch_strategy.py:1433:57: error[unresolved-attribute] Object of type `Self@mirror_id` has no attribute `revision`
+ lib/spack/spack/fetch_strategy.py:1547:19: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/graph.py:98:9: warning[possibly-missing-attribute] Attribute `write` may be missing on object of type `Unknown | None`
+ lib/spack/spack/graph.py:103:16: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/graph.py:105:16: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/graph.py:106:20: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/graph.py:107:9: warning[possibly-missing-attribute] Attribute `write` may be missing on object of type `Unknown | None`
+ lib/spack/spack/graph.py:131:31: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `Unknown | None | list[Unknown | list[Unknown]]`
+ lib/spack/spack/graph.py:132:17: warning[possibly-missing-attribute] Attribute `index` may be missing on object of type `Unknown | None | list[Unknown | list[Unknown]]`
+ lib/spack/spack/graph.py:136:17: warning[possibly-missing-attribute] Attribute `pop` may be missing on object of type `Unknown | None | list[Unknown | list[Unknown]]`
+ lib/spack/spack/graph.py:137:17: warning[possibly-missing-attribute] Attribute `insert` may be missing on object of type `Unknown | None | list[Unknown | list[Unknown]]`
+ lib/spack/spack/graph.py:160:13: warning[possibly-missing-attribute] Attribute `insert` may be missing on object of type `Unknown | None | list[Unknown | list[Unknown]]`
+ lib/spack/spack/graph.py:170:13: warning[possibly-missing-attribute] Attribute `write` may be missing on object of type `Unknown | None`
+ lib/spack/spack/graph.py:171:13: warning[possibly-missing-attribute] Attribute `write` may be missing on object of type `Unknown | None`
+ lib/spack/spack/graph.py:172:13: warning[possibly-missing-attribute] Attribute `write` may be missing on object of type `Unknown | None`
+ lib/spack/spack/graph.py:173:13: warning[possibly-missing-attribute] Attribute `write` may be missing on object of type `Unknown | None`
+ lib/spack/spack/graph.py:219:28: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `SupportsIndex`, found `Unknown | None | Literal[0]`
+ lib/spack/spack/graph.py:222:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `None` and `Literal[1]`
+ lib/spack/spack/graph.py:224:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | None | list[Unknown | list[Unknown]]`
+ lib/spack/spack/graph.py:247:9: warning[possibly-missing-attribute] Attribute `write` may be missing on object of type `Unknown | None`
+ lib/spack/spack/graph.py:258:9: warning[possibly-missing-attribute] Attribute `write` may be missing on object of type `Unknown | None`
+ lib/spack/spack/graph.py:260:39: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | None | list[Unknown | list[Unknown]]`
+ lib/spack/spack/graph.py:263:9: warning[possibly-missing-attribute] Attribute `write` may be missing on object of type `Unknown | None`
+ lib/spack/spack/graph.py:265:9: warning[possibly-missing-attribute] Attribute `write` may be missing on object of type `Unknown | None`
+ lib/spack/spack/graph.py:272:35: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | None | list[Unknown | list[Unknown]]`
+ lib/spack/spack/graph.py:276:9: warning[possibly-missing-attribute] Attribute `write` may be missing on object of type `Unknown | None`
+ lib/spack/spack/graph.py:285:39: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | None | list[Unknown | list[Unknown]]`
+ lib/spack/spack/graph.py:289:9: warning[possibly-missing-attribute] Attribute `write` may be missing on object of type `Unknown | None`
+ lib/spack/spack/graph.py:299:39: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | None | list[Unknown | list[Unknown]]`
+ lib/spack/spack/graph.py:303:9: warning[possibly-missing-attribute] Attribute `write` may be missing on object of type `Unknown | None`
- lib/spack/spack/install_test.py:409:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/installer.py:1299:74: warning[possibly-missing-attribute] Attribute `poll` may be missing on object of type `Unknown | None`
+ lib/spack/spack/installer.py:1365:55: warning[possibly-missing-attribute] Attribute `complete` may be missing on object of type `Unknown | None`
+ lib/spack/spack/llnl/util/lock.py:767:24: error[call-non-callable] Object of type `ContextManager[Unknown]` is not callable
+ lib/spack/spack/llnl/util/tty/color.py:428:32: error[too-many-positional-arguments] Too many positional arguments to bound method `write`: expected 2, got 3
+ lib/spack/spack/llnl/util/tty/color.py:428:32: error[unresolved-attribute] Object of type `Self@writelines` has no attribute `color`
+ lib/spack/spack/llnl/util/tty/log.py:590:21: error[invalid-argument-type] Argument to function `dup2` is incorrect: Expected `int`, found `Unknown | int | TextIO`
+ lib/spack/spack/llnl/util/tty/log.py:591:22: error[invalid-argument-type] Argument to function `close` is incorrect: Expected `int`, found `Unknown | int | TextIO`
+ lib/spack/spack/llnl/util/tty/log.py:593:21: error[invalid-argument-type] Argument to function `dup2` is incorrect: Expected `int`, found `Unknown | int | TextIO`
+ lib/spack/spack/llnl/util/tty/log.py:594:22: error[invalid-argument-type] Argument to function `close` is incorrect: Expected `int`, found `Unknown | int | TextIO`
+ lib/spack/spack/llnl/util/tty/log.py:603:13: warning[possibly-missing-attribute] Attribute `write` may be missing on object of type `Unknown | None`
+ lib/spack/spack/llnl/util/tty/pty.py:249:9: warning[possibly-missing-attribute] Attribute `join` may be missing on object of type `Unknown | None | Process`
+ lib/spack/spack/llnl/util/tty/pty.py:250:16: warning[possibly-missing-attribute] Attribute `exitcode` may be missing on object of type `Unknown | None | Process`
+ lib/spack/spack/mirrors/mirror.py:263:45: error[invalid-argument-type] Argument to bound method `_update_connection_dict` is incorrect: Expected `dict[Unknown, Unknown]`, found `@Todo | str`
+ lib/spack/spack/multimethod.py:137:42: error[unresolved-attribute] Object of type `Self@__call__` has no attribute `__name__`
+ lib/spack/spack/multimethod.py:149:13: error[unresolved-attribute] Object of type `Self@__call__` has no attribute `__name__`
+ lib/spack/spack/package_base.py:253:33: error[unresolved-attribute] Object of type `(Self@__init__ & <Protocol with members 'executables'> & ~<Protocol with members 'libraries'>) | (Self@__init__ & <Protocol with members 'libraries'> & ~<Protocol with members 'executables'>)` has no attribute `namespace`
+ lib/spack/spack/package_base.py:253:55: error[unresolved-attribute] Object of type `(Self@__init__ & <Protocol with members 'executables'> & ~<Protocol with members 'libraries'>) | (Self@__init__ & <Protocol with members 'libraries'> & ~<Protocol with members 'executables'>)` has no attribute `name`
+ lib/spack/spack/package_base.py:266:48: error[unresolved-attribute] Object of type `(Self@__init__ & <Protocol with members 'executables'> & ~<Protocol with members 'libraries'> & ~<Protocol with members 'determine_version'>) | (Self@__init__ & <Protocol with members 'libraries'> & ~<Protocol with members 'executables'> & ~<Protocol with members 'determine_version'>)` has no attribute `name`
+ lib/spack/spack/package_base.py:266:58: error[unresolved-attribute] Object of type `(Self@__init__ & <Protocol with members 'executables'> & ~<Protocol with members 'libraries'> & ~<Protocol with members 'determine_version'>) | (Self@__init__ & <Protocol with members 'libraries'> & ~<Protocol with members 'executables'> & ~<Protocol with members 'determine_version'>)` has no attribute `namespace`
+ lib/spack/spack/package_base.py:347:16: error[unresolved-attribute] Object of type `Self@view_source` has no attribute `spec`
+ lib/spack/spack/package_base.py:353:45: error[unresolved-attribute] Object of type `Self@view_destination` has no attribute `spec`
+ lib/spack/spack/package_base.py:380:46: error[unresolved-attribute] Object of type `Self@add_files_to_view` has no attribute `spec`
+ lib/spack/spack/package_base.py:383:42: error[unresolved-attribute] Object of type `Self@add_files_to_view` has no attribute `spec`
+ lib/spack/spack/package_base.py:1768:21: error[unresolved-attribute] Object of type `Self@do_patch` has no attribute `patch`
+ lib/spack/spack/provider_index.py:51:20: error[unresolved-attribute] Object of type `str` has no attribute `intersects`
+ lib/spack/spack/provider_index.py:159:63: error[unresolved-attribute] Object of type `str` has no attribute `name`
+ lib/spack/spack/provider_index.py:211:52: error[unresolved-attribute] Object of type `str` has no attribute `fullname`
+ lib/spack/spack/repo.py:489:9: warning[possibly-missing-attribute] Attribute `update_package` may be missing on object of type `Unknown | None`
+ lib/spack/spack/repo.py:492:9: warning[possibly-missing-attribute] Attribute `to_json` may be missing on object of type `Unknown | None`
+ lib/spack/spack/repo.py:511:9: warning[possibly-missing-attribute] Attribute `remove_provider` may be missing on object of type `Unknown | None`
+ lib/spack/spack/repo.py:512:9: warning[possibly-missing-attribute] Attribute `update` may be missing on object of type `Unknown | None`
+ lib/spack/spack/repo.py:515:9: warning[possibly-missing-attribute] Attribute `to_json` may be missing on object of type `Unknown | None`
+ lib/spack/spack/repo.py:535:9: warning[possibly-missing-attribute] Attribute `to_json` may be missing on object of type `Unknown | None`
+ lib/spack/spack/repo.py:538:9: warning[possibly-missing-attribute] Attribute `update_package` may be missing on object of type `Unknown | None`
- lib/spack/spack/solver/asp.py:838:12: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/spec.py:2224:34: error[unresolved-attribute] Object of type `Self@package_hash` has no attribute `_package_hash`
+ lib/spack/spack/spec.py:3111:21: warning[possibly-missing-attribute] Attribute `intersects` may be missing on object of type `Unknown | None | ArchSpec`
+ lib/spack/spack/spec.py:3132:24: warning[possibly-missing-attribute] Attribute `constrain` may be missing on object of type `Unknown | None | ArchSpec`
+ lib/spack/spack/spec.py:5465:17: error[call-non-callable] Object of type `None` is not callable
+ lib/spack/spack/spec.py:5465:38: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ lib/spack/spack/spec_parser.py:370:32: error[invalid-argument-type] Argument to bound method `accept` is incorrect: Expected `SpecTokens`, found `Unknown | Literal["(?:[\\^%]\\[)"]`
+ lib/spack/spack/spec_parser.py:371:29: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Unknown | None`
+ lib/spack/spack/spec_parser.py:391:34: error[invalid-argument-type] Argument to bound method `accept` is incorrect: Expected `SpecTokens`, found `Unknown | Literal["(?:[\\^\\%](?:\\s*(?:(?P<virtuals>(?:[a-zA-Z_0-9][a-zA-Z_0-9\\-]*)(?:,(?:[a-zA-Z_0-9][a-zA-Z_0-9\\-]*))*)=(?P<substitute>(?:(?:[a-zA-Z_0-9][a-zA-Z_0-9\\-]*)(?:\\.(?:[a-zA-Z_0-9][a-zA-Z_0-9\\-]*))+)|(?:[a-zA-Z_0-9][a-zA-Z_0-9\\-]*))))?)"]`
+ lib/spack/spack/spec_parser.py:392:29: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Unknown | None`
+ lib/spack/spack/stage.py:615:54: warning[possibly-missing-attribute] Attribute `path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/subprocess_context.py:94:13: warning[possibly-missing-attribute] Attribute `restore` may be missing on object of type `Unknown | GlobalStateMarshaler | None`
+ lib/spack/spack/subprocess_context.py:95:13: warning[possibly-missing-attribute] Attribute `restore` may be missing on object of type `Unknown | None`
+ lib/spack/spack/tengine.py:63:54: error[unresolved-attribute] Object of type `Self@to_dict` has no attribute `context_properties`
+ lib/spack/spack/test/conftest.py:2291:16: warning[possibly-missing-attribute] Attribute `read` may be missing on object of type `Unknown | None`
+ lib/spack/spack/util/archive.py:40:13: warning[possibly-missing-attribute] Attribute `write` may be missing on object of type `Unknown | None`
+ lib/spack/spack/util/archive.py:64:9: warning[possibly-missing-attribute] Attribute `close` may be missing on object of type `Unknown | None`
+ lib/spack/spack/util/archive.py:68:9: warning[possibly-missing-attribute] Attribute `flush` may be missing on object of type `Unknown | None`
+ lib/spack/spack/util/archive.py:71:16: warning[possibly-missing-attribute] Attribute `fileno` may be missing on object of type `Unknown | None`
+ lib/spack/spack/util/archive.py:86:16: warning[possibly-missing-attribute] Attribute `tell` may be missing on object of type `Unknown | None`
+ lib/spack/spack/util/debug.py:71:50: error[invalid-argument-type] Argument to function `fdopen` is incorrect: Expected `int`, found `Unknown | int | None`
+ lib/spack/spack/util/gcs.py:92:20: warning[possibly-missing-attribute] Attribute `get_blob` may be missing on object of type `Unknown | None`
+ lib/spack/spack/util/gcs.py:97:20: warning[possibly-missing-attribute] Attribute `blob` may be missing on object of type `Unknown | None`
+ lib/spack/spack/util/gcs.py:116:25: warning[possibly-missing-attribute] Attribute `list_blobs` may be missing on object of type `Unknown | None`
+ lib/spack/spack/util/s3.py:140:9: error[invalid-assignment] Object of type `None` is not assignable to attribute `raw` of type `_BufferedReaderStream`
+ lib/spack/spack/util/s3.py:143:16: warning[possibly-missing-attribute] Attribute `read` may be missing on object of type `_BufferedReaderStream | Unknown | None`
+ lib/spack/spack/vendor/attr/_make.py:2610:65: error[unresolved-attribute] Object of type `Self@__getstate__` has no attribute `metadata`
+ lib/spack/spack/vendor/jinja2/environment.py:874:38: error[invalid-argument-type] Argument to function `write_file` is incorrect: Expected `str`, found `CodeType`
- lib/spack/spack/vendor/jinja2/environment.py:1310:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/environment.py:1364:61: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/environment.py:1414:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/environment.py:1607:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/ext.py:325:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/ext.py:326:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/ext.py:520:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/idtracking.py:154:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/lexer.py:667:40: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | Unknown | int`
+ lib/spack/spack/vendor/jinja2/lexer.py:667:40: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | int | Any`
- lib/spack/spack/vendor/jinja2/loaders.py:348:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/loaders.py:386:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/nativetypes.py:118:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/nodes.py:214:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/parser.py:155:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/vendor/jinja2/parser.py:433:9: error[invalid-assignment] Object of type `Expr` is not assignable to attribute `call` of type `Call`
- lib/spack/spack/vendor/jinja2/parser.py:317:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/parser.py:439:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/parser.py:515:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/parser.py:792:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/vendor/jinja2/parser.py:1023:37: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[Node]`, found `(Node & Top[list[Unknown]]) | list[Node]`
- lib/spack/spack/vendor/jinja2/runtime.py:417:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/runtime.py:428:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/runtime.py:817:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/vendor/jinja2/runtime.py:814:40: error[invalid-argument-type] Argument to bound method `_invoke` is incorrect: Expected `bool`, found `@Todo | bool | None`
- lib/spack/spack/vendor/jinja2/runtime.py:826:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/vendor/jsonschema/exceptions.py:83:13: error[unresolved-attribute] Object of type `Self@__unicode__` has no attribute `_word_for_schema_in_error_message`
+ lib/spack/spack/vendor/jsonschema/exceptions.py:86:13: error[unresolved-attribute] Object of type `Self@__unicode__` has no attribute `_word_for_instance_in_error_message`
+ lib/spack/spack/vendor/macholib/MachO.py:358:49: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | None | list[Unknown]`
+ lib/spack/spack/vendor/macholib/MachO.py:400:29: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/vendor/macholib/MachO.py:405:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | list[Unknown]` may be missing
+ lib/spack/spack/vendor/macholib/MachO.py:411:13: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | None` and `Unknown | None | Literal[0]`
+ lib/spack/spack/vendor/macholib/MachO.py:419:21: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | None` and `Unknown | None | Literal[0]`
+ lib/spack/spack/vendor/macholib/MachO.py:424:9: warning[possibly-missing-attribute] Attribute `sizeofcmds` may be missing on object of type `Unknown | None`
+ lib/spack/spack/vendor/macholib/MachO.py:425:54: warning[possibly-missing-attribute] Attribute `sizeofcmds` may be missing on object of type `Unknown | None`
+ lib/spack/spack/vendor/macholib/MachO.py:435:9: warning[possibly-missing-attribute] Attribute `to_fileobj` may be missing on object of type `Unknown | None`
+ lib/spack/spack/vendor/macholib/MachO.py:436:30: error[not-iterable] Object of type `Unknown | None | list[Unknown]` may not be iterable
+ lib/spack/spack/vendor/macholib/MachO.py:467:31: error[not-iterable] Object of type `Unknown | None | list[Unknown]` may not be iterable
+ lib/spack/spack/vendor/macholib/MachO.py:473:31: error[not-iterable] Object of type `Unknown | None | list[Unknown]` may not be iterable
+ lib/spack/spack/vendor/macholib/ptypes.py:63:44: error[unresolved-attribute] Object of type `Self@from_mmap` has no attribute `_size_`
+ lib/spack/spack/vendor/macholib/ptypes.py:66:36: error[unresolved-attribute] Object of type `Self@from_fileobj` has no attribute `_size_`
+ lib/spack/spack/vendor/macholib/ptypes.py:69:37: error[unresolved-attribute] Object of type `Self@from_str` has no attribute `_endian_`
+ lib/spack/spack/vendor/macholib/ptypes.py:70:54: error[unresolved-attribute] Object of type `Self@from_str` has no attribute `_format_`
+ lib/spack/spack/vendor/macholib/ptypes.py:86:24: error[unresolved-attribute] Object of type `Self@to_mmap` has no attribute `_size_`
+ lib/spack/spack/vendor/macholib/ptypes.py:188:35: error[unresolved-attribute] Object of type `Self@from_tuple` has no attribute `_structmarks_`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:91:22: error[unresolved-attribute] Object of type `Self@__iter__` has no attribute `_left_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:91:39: error[unresolved-attribute] Object of type `Self@__iter__` has no attribute `_right_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:95:62: error[unresolved-attribute] Object of type `Self@__repr__` has no attribute `_maxlen`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:95:79: error[unresolved-attribute] Object of type `Self@__repr__` has no attribute `_maxlen`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:95:79: error[unresolved-attribute] Object of type `Self@__repr__` has no attribute `_maxlen`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:95:79: error[unresolved-attribute] Object of type `Self@__repr__` has no attribute `_maxlen`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:122:59: error[unresolved-attribute] Object of type `Self@pop` has no attribute `_right_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:122:77: error[unresolved-attribute] Object of type `Self@pop` has no attribute `_left_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:123:58: error[unresolved-attribute] Object of type `Self@pop` has no attribute `_length`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:123:84: error[unresolved-attribute] Object of type `Self@pop` has no attribute `_maxlen`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:136:59: error[unresolved-attribute] Object of type `Self@popleft` has no attribute `_left_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:136:76: error[unresolved-attribute] Object of type `Self@popleft` has no attribute `_right_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:137:58: error[unresolved-attribute] Object of type `Self@popleft` has no attribute `_length`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:137:84: error[unresolved-attribute] Object of type `Self@popleft` has no attribute `_maxlen`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:158:20: error[unresolved-attribute] Object of type `Self@_is_empty` has no attribute `_left_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:158:44: error[unresolved-attribute] Object of type `Self@_is_empty` has no attribute `_right_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:181:16: error[unresolved-attribute] Object of type `Self@__len__` has no attribute `_length`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:190:66: error[unresolved-attribute] Object of type `Self@append` has no attribute `_left_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:190:83: error[unresolved-attribute] Object of type `Self@append` has no attribute `_right_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:191:66: error[unresolved-attribute] Object of type `Self@append` has no attribute `_maxlen`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:200:66: error[unresolved-attribute] Object of type `Self@appendleft` has no attribute `_right_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:200:84: error[unresolved-attribute] Object of type `Self@appendleft` has no attribute `_left_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:201:66: error[unresolved-attribute] Object of type `Self@appendleft` has no attribute `_maxlen`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:204:12: error[unresolved-attribute] Object of type `Self@_append` has no attribute `_maxlen`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:204:41: error[unresolved-attribute] Object of type `Self@_append` has no attribute `_length`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:204:57: error[unresolved-attribute] Object of type `Self@_append` has no attribute `_maxlen`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:205:16: error[unresolved-attribute] Object of type `Self@_append` has no attribute `_maxlen`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:208:69: error[unresolved-attribute] Object of type `Self@_append` has no attribute `_length`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:210:57: error[unresolved-attribute] Object of type `Self@_append` has no attribute `_length`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:224:23: error[unresolved-attribute] Object of type `Self@_extend` has no attribute `_length`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:225:12: error[unresolved-attribute] Object of type `Self@_extend` has no attribute `_maxlen`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:225:55: error[unresolved-attribute] Object of type `Self@_extend` has no attribute `_maxlen`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:226:37: error[unresolved-attribute] Object of type `Self@_extend` has no attribute `_maxlen`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:239:68: error[unresolved-attribute] Object of type `Self@extend` has no attribute `_right_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:239:86: error[unresolved-attribute] Object of type `Self@extend` has no attribute `_left_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:240:54: error[unresolved-attribute] Object of type `Self@extend` has no attribute `_length`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:240:83: error[unresolved-attribute] Object of type `Self@extend` has no attribute `_maxlen`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:251:68: error[unresolved-attribute] Object of type `Self@extendleft` has no attribute `_left_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:251:85: error[unresolved-attribute] Object of type `Self@extendleft` has no attribute `_right_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:252:54: error[unresolved-attribute] Object of type `Self@extendleft` has no attribute `_length`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:252:83: error[unresolved-attribute] Object of type `Self@extendleft` has no attribute `_maxlen`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:261:16: error[unresolved-attribute] Object of type `Self@count` has no attribute `_left_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:261:46: error[unresolved-attribute] Object of type `Self@count` has no attribute `_right_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:272:27: error[unresolved-attribute] Object of type `Self@remove` has no attribute `_left_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:272:57: error[unresolved-attribute] Object of type `Self@remove` has no attribute `_right_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:272:75: error[unresolved-attribute] Object of type `Self@remove` has no attribute `_length`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:277:31: error[unresolved-attribute] Object of type `Self@remove` has no attribute `_left_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:278:32: error[unresolved-attribute] Object of type `Self@remove` has no attribute `_right_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:278:83: error[unresolved-attribute] Object of type `Self@remove` has no attribute `_length`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:294:23: error[unresolved-attribute] Object of type `Self@reverse` has no attribute `_right_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:294:41: error[unresolved-attribute] Object of type `Self@reverse` has no attribute `_left_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:294:58: error[unresolved-attribute] Object of type `Self@reverse` has no attribute `_length`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:315:37: error[unresolved-attribute] Object of type `Self@__reduce__` has no attribute `_maxlen`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:321:58: error[unresolved-attribute] Object of type `Self@__getitem__` has no attribute `_maxlen`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:325:55: error[unresolved-attribute] Object of type `Self@__getitem__` has no attribute `_length`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:327:37: error[unresolved-attribute] Object of type `Self@__getitem__` has no attribute `_length`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:327:66: error[unresolved-attribute] Object of type `Self@__getitem__` has no attribute `_length`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:22:13: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `rest` on type `(Unknown & ~AlwaysFalsy) | (_EmptyPList & ~AlwaysFalsy)`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:103:34: error[unresolved-attribute] Object of type `Self@reverse & ~AlwaysFalsy` has no attribute `first`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:104:20: error[unresolved-attribute] Object of type `Self@reverse & ~AlwaysFalsy` has no attribute `rest`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:121:28: error[unresolved-attribute] Object of type `Self@split & ~AlwaysFalsy` has no attribute `first`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:122:26: error[unresolved-attribute] Object of type `Self@split & ~AlwaysFalsy` has no attribute `rest`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:134:19: error[unresolved-attribute] Object of type `Self@__iter__ & ~AlwaysFalsy` has no attribute `first`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:135:18: error[unresolved-attribute] Object of type `Self@__iter__ & ~AlwaysFalsy` has no attribute `rest`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:155:20: error[unresolved-attribute] Object of type `Self@__eq__ & ~AlwaysFalsy` has no attribute `first`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:157:25: error[unresolved-attribute] Object of type `Self@__eq__ & ~AlwaysFalsy` has no attribute `rest`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:191:20: error[unresolved-attribute] Object of type `Self@_drop` has no attribute `rest`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:213:16: error[unresolved-attribute] Object of type `Self@remove & ~AlwaysFalsy` has no attribute `first`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:214:45: error[unresolved-attribute] Object of type `Self@remove & ~AlwaysFalsy` has no attribute `rest`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:216:33: error[unresolved-attribute] Object of type `Self@remove & ~AlwaysFalsy` has no attribute `first`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:217:20: error[unresolved-attribute] Object of type `Self@remove & ~AlwaysFalsy` has no attribute `rest`
+ lib/spack/spack/vendor/pyrsistent/_pset.py:36:27: error[unresolved-attribute] Object of type `Self@__contains__` has no attribute `_map`
+ lib/spack/spack/vendor/pyrsistent/_pset.py:39:21: error[unresolved-attribute] Object of type `Self@__iter__` has no attribute `_map`
+ lib/spack/spack/vendor/pyrsistent/_pset.py:42:20: error[unresolved-attribute] Object of type `Self@__len__` has no attribute `_map`
+ lib/spack/spack/vendor/pyrsistent/_pset.py:54:21: error[unresolved-attribute] Object of type `Self@__hash__` has no attribute `_map`
+ lib/spack/spack/vendor/pyrsistent/_pset.py:96:23: error[unresolved-attribute] Object of type `Self@remove` has no attribute `_map`
+ lib/spack/spack/vendor/pyrsistent/_pset.py:105:23: error[unresolved-attribute] Object of type `Self@discard` has no attribute `_map`
- lib/spack/spack/vendor/ruamel/yaml/comments.py:361:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/ruamel/yaml/compat.py:218:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/vendor/ruamel/yaml/constructor.py:191:50: error[too-many-positional-arguments] Too many positional arguments to func...*[Comment body truncated]*

---

_@sharkdp reviewed on 2025-10-22 14:54_

---

_Review comment by @sharkdp on `python/py-fuzzer/fuzz.py`:142 on 2025-10-22 14:54_

FYI @AlexWaygood :smile: 

---

_@sharkdp reviewed on 2025-10-22 14:58_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:92 on 2025-10-22 14:58_

This (and below) was really only "working" because we didn't infer a type of `self`, not because we ever added support for this. As far as I remember, no other type checker supports this either and we decided at the time that it was not important to implement.

---

_@AlexWaygood reviewed on 2025-10-22 14:59_

---

_Review comment by @AlexWaygood on `python/py-fuzzer/fuzz.py`:142 on 2025-10-22 14:59_

Let's go!

---

_Marked ready for review by @sharkdp on 2025-10-22 15:47_

---

_Review requested from @carljm by @sharkdp on 2025-10-22 15:47_

---

_Review requested from @dcreager by @sharkdp on 2025-10-22 15:47_

---

_Review requested from @MichaReiser by @sharkdp on 2025-10-22 15:47_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/builder.rs`:209 on 2025-10-22 16:25_

Just curious, what's the reason for reducing the count here?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:2547 on 2025-10-22 16:27_

Nit: Can we use `self.file()` here? All scopes should belong to the same file

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:2553 on 2025-10-22 16:29_

I'm not sure if this is still the case or if I remember it incorrectly. 

For generic classes, isn't there a nested scope for the generic between the class and its members?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:2553 on 2025-10-22 16:30_

Nit: I'd find an explicit return here easier to read
```suggestion
        if !parent_scope.kind().is_class() {
        	return;
      	}
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:2570 on 2025-10-22 16:33_

Should we also return here if the function has no parameter named `parameter.name` (which should never happen?)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:2582 on 2025-10-22 16:34_

Nit: Not sure if it's worth it but we already check that the parent is a class on the line above. Could we infer the type directly instead of walking the ancestor chain again?

---

_@MichaReiser approved on 2025-10-22 16:35_

Congrats! 

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer/builder.rs`:2553 on 2025-10-22 17:25_

I think it's the other way around — the generic scope contains the class scope, which then contains the class members

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer/builder.rs`:2565 on 2025-10-22 17:29_

Can this be `self.module()`?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer/builder.rs`:2570 on 2025-10-22 17:31_

Or alternatively, can we pass the parameter index into `infer_parameter_definition`, and only call this method for the first parameter? Then we don't have to look up by name, and we don't have to verify that what we found was parameter 0.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer/builder.rs`:2582 on 2025-10-22 17:36_

Or rather, the check above will only pass when the function containing this parameter is an immediate child of a class. It won't work for nested functions. _Explicit_ `Self` is allowed in a nested function ([spec](https://typing.python.org/en/latest/spec/generics.html#valid-locations-for-self), `HasNestedFunction.foo.nested`). But is _implicit_ `Self` allowed there? i.e., should this work?

```py
class C:
    def f(self):
        def nested(self):
            reveal_type(self)  # revealed: Self@C
```

If so, then the call above should use `nearest_enclosing_class`. If not, then I agree with Micha that here we just use the immediately enclosing class that we can get from above.

---

_@dcreager approved on 2025-10-22 17:36_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:2553 on 2025-10-22 17:50_

Yes, but this is still a good catch. Because there can be a generic scope between class body and function body if the *method* itself is generic. We made that error in a few other places, and I thought I had them all in my mind (and we're fixing them in https://github.com/astral-sh/ruff/pull/20856), but didn't remember that we introduce this error again here.

---

_@sharkdp reviewed on 2025-10-22 17:50_

---

_@sharkdp reviewed on 2025-10-23 06:51_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:2570 on 2025-10-23 06:51_

> can we pass the parameter index into infer_parameter_definition

I see no way of doing that, unfortunately. The AST doesn't store the index of the parameter.

---

_@sharkdp reviewed on 2025-10-23 06:55_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/builder.rs`:209 on 2025-10-23 06:55_

Sorry, this was discussed on the original PR. It was necessary to prevent ecosystem panics. The true fix for this is https://github.com/astral-sh/ty/issues/957.

---

_@sharkdp reviewed on 2025-10-23 07:07_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:2582 on 2025-10-23 07:07_

Thanks, I added better tests and removed the `nearest_enclosing_class` call.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-23 07:08_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-23 07:08_

---

_Merged by @sharkdp on 2025-10-23 07:34_

---

_Closed by @sharkdp on 2025-10-23 07:34_

---

_Branch deleted on 2025-10-23 07:34_

---

_@dcreager reviewed on 2025-10-23 13:14_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer/builder.rs`:2570 on 2025-10-23 13:14_

It looks like [`element_offset`](https://doc.rust-lang.org/std/primitive.slice.html#method.element_offset) is [stabilizing soon](https://github.com/rust-lang/rust/issues/126769#issuecomment-3418757954) — so in some number of months we'll be able to get the parameter index from the function's parameter slice without threading it through explicitly!

---
