```yaml
number: 20107
title: "[ty] support PEP 613 type aliases"
type: pull_request
state: closed
author: carljm
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: cjm/pep613alias
created_at: 2025-08-27T00:33:13Z
updated_at: 2025-11-20T04:54:28Z
url: https://github.com/astral-sh/ruff/pull/20107
synced_at: 2026-01-12T15:56:54Z
```

# [ty] support PEP 613 type aliases

---

_@carljm_

## Summary

Add support for PEP 613 type aliases (defined by annotating with `typing.TypeAlias`), including recursive ones.

## Test Plan

Added mdtests and updated existing tests.


---

_Label `ty` added by @carljm on 2025-08-27 00:33_

---

_Comment by @github-actions[bot] on 2025-08-27 00:35_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-16 12:51:07.719997664 +0000
+++ new-output.txt	2025-10-16 12:51:11.082995592 +0000
@@ -1,27 +1,34 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/execute.rs:402:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(d417)): execute: too many cycle iterations`
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/execute.rs:402:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(16c43)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/execute.rs:402:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAlias < 'db >::value_type_(Id(d817)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/execute.rs:402:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(17043)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
 _directives_deprecated_library.py:41:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | float`
 _directives_deprecated_library.py:45:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 aliases_explicit.py:41:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
-aliases_explicit.py:45:10: error[invalid-type-form] Variable of type `Literal["int | str"]` is not allowed in a type expression
-aliases_explicit.py:49:5: error[type-assertion-failure] Argument does not have asserted type `int | str`
-aliases_explicit.py:50:5: error[type-assertion-failure] Argument does not have asserted type `int | None`
-aliases_explicit.py:51:5: error[type-assertion-failure] Argument does not have asserted type `list[int | None]`
 aliases_explicit.py:52:5: error[type-assertion-failure] Argument does not have asserted type `list[int]`
 aliases_explicit.py:53:5: error[type-assertion-failure] Argument does not have asserted type `tuple[str, ...] | list[str]`
 aliases_explicit.py:54:5: error[type-assertion-failure] Argument does not have asserted type `tuple[int, int, int, str]`
-aliases_explicit.py:55:5: error[type-assertion-failure] Argument does not have asserted type `(...) -> int`
 aliases_explicit.py:56:5: error[type-assertion-failure] Argument does not have asserted type `(int, str, /) -> str`
 aliases_explicit.py:57:5: error[type-assertion-failure] Argument does not have asserted type `(int, str, str, /) -> None`
 aliases_explicit.py:59:5: error[type-assertion-failure] Argument does not have asserted type `int | str | None | list[list[int]]`
-aliases_explicit.py:60:5: error[type-assertion-failure] Argument does not have asserted type `(...) -> None`
-aliases_explicit.py:61:5: error[type-assertion-failure] Argument does not have asserted type `int | str`
-aliases_explicit.py:63:5: error[type-assertion-failure] Argument does not have asserted type `Literal[3, 4, 5] | None`
+aliases_explicit.py:79:21: error[invalid-type-form] Function calls are not allowed in type expressions
+aliases_explicit.py:80:21: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
+aliases_explicit.py:81:21: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression
+aliases_explicit.py:81:22: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
+aliases_explicit.py:82:21: error[invalid-type-form] List comprehensions are not allowed in type expressions
+aliases_explicit.py:83:21: error[invalid-type-form] Dict literals are not allowed in type expressions
+aliases_explicit.py:84:21: error[invalid-type-form] Function calls are not allowed in type expressions
+aliases_explicit.py:85:27: error[invalid-type-form] Int literals are not allowed in this context in a type expression
+aliases_explicit.py:86:21: error[invalid-type-form] `if` expressions are not allowed in type expressions
+aliases_explicit.py:87:21: error[invalid-type-form] Variable of type `Literal[3]` is not allowed in a type expression
+aliases_explicit.py:88:22: error[invalid-type-form] Boolean literals are not allowed in this context in a type expression
+aliases_explicit.py:89:22: error[invalid-type-form] Int literals are not allowed in this context in a type expression
+aliases_explicit.py:90:22: error[invalid-type-form] Boolean operations are not allowed in type expressions
+aliases_explicit.py:91:22: error[invalid-type-form] F-strings are not allowed in type expressions
+aliases_explicit.py:97:17: error[call-non-callable] Object of type `object` is not callable
 aliases_explicit.py:98:1: error[type-assertion-failure] Argument does not have asserted type `list[str]`
-aliases_explicit.py:101:6: error[call-non-callable] Object of type `UnionType` is not callable
+aliases_explicit.py:101:6: error[call-non-callable] Object of type `object` is not callable
 aliases_implicit.py:54:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
 aliases_implicit.py:60:5: error[type-assertion-failure] Argument does not have asserted type `int | str`
 aliases_implicit.py:61:5: error[type-assertion-failure] Argument does not have asserted type `int | None`
@@ -48,10 +55,11 @@
 aliases_implicit.py:133:6: error[call-non-callable] Object of type `UnionType` is not callable
 aliases_newtype.py:15:1: error[type-assertion-failure] Argument does not have asserted type `int`
 aliases_newtype.py:18:1: error[invalid-assignment] Object of type `NewType` is not assignable to `type`
+aliases_newtype.py:23:16: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `NewType`
 aliases_newtype.py:26:21: error[invalid-base] Invalid class base with type `NewType`
 aliases_newtype.py:63:43: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 3, got 4
-aliases_variance.py:18:24: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[typing.TypeVar]'>` with no `__class_getitem__` method
-aliases_variance.py:28:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[typing.TypeVar]'>` with no `__class_getitem__` method
+aliases_variance.py:28:16: error[non-subscriptable] Cannot subscript object of type `A_Alias_1` with no `__getitem__` method
+aliases_variance.py:32:16: error[non-subscriptable] Cannot subscript object of type `A_Alias_2` with no `__getitem__` method
 aliases_variance.py:44:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassB[typing.TypeVar, typing.TypeVar]'>` with no `__class_getitem__` method
 annotations_forward_refs.py:49:10: error[invalid-type-form] Variable of type `Literal[1]` is not allowed in a type expression
 annotations_forward_refs.py:54:11: error[fstring-type-annotation] Type expressions cannot use f-strings
@@ -166,6 +174,7 @@
 classes_classvar.py:71:18: error[invalid-type-form] `ClassVar` annotations are not allowed for non-name targets
 classes_classvar.py:73:26: error[invalid-type-form] `ClassVar` is not allowed in function return type annotations
 classes_classvar.py:77:8: error[invalid-type-form] `ClassVar` annotations are only allowed in class-body scopes
+classes_classvar.py:78:20: error[invalid-type-form] Type qualifier `typing.ClassVar` is not allowed in type expressions (only in annotation expressions)
 classes_classvar.py:111:1: error[invalid-attribute-access] Cannot assign to ClassVar `stats` from an instance of type `Starship`
 classes_classvar.py:140:1: error[invalid-assignment] Object of type `ProtoAImpl` is not assignable to `ProtoA`
 constructors_call_init.py:21:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `float`
@@ -399,7 +408,7 @@
 generics_defaults_referential.py:95:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
 generics_defaults_specialization.py:26:5: error[type-assertion-failure] Argument does not have asserted type `SomethingWithNoDefaults[int, str]`
 generics_defaults_specialization.py:27:5: error[type-assertion-failure] Argument does not have asserted type `SomethingWithNoDefaults[int, bool]`
-generics_defaults_specialization.py:30:1: error[non-subscriptable] Cannot subscript object of type `<class 'SomethingWithNoDefaults[int, typing.TypeVar]'>` with no `__class_getitem__` method
+generics_defaults_specialization.py:30:1: error[non-subscriptable] Cannot subscript object of type `MyAlias` with no `__getitem__` method
 generics_defaults_specialization.py:45:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
 generics_paramspec_basic.py:27:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_components.py:49:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `tuple[@Todo(Support for `typing.ParamSpec`), ...]`
@@ -799,7 +808,9 @@
 qualifiers_annotated.py:53:18: error[invalid-type-form] Boolean operations are not allowed in type expressions
 qualifiers_annotated.py:54:18: error[fstring-type-annotation] Type expressions cannot use f-strings
 qualifiers_annotated.py:64:8: error[invalid-type-form] Special form `typing.Annotated` expected at least 2 arguments (one type and at least one metadata element)
+qualifiers_annotated.py:77:1: error[invalid-assignment] Object of type `SmallInt` is not assignable to `type[Any]`
 qualifiers_annotated.py:91:1: error[call-non-callable] Object of type `typing.Annotated` is not callable
+qualifiers_annotated.py:93:1: error[call-non-callable] Object of type `object` is not callable
 qualifiers_final_annotation.py:18:7: error[invalid-type-form] Type qualifier `typing.Final` expected exactly 1 argument, got 2
 qualifiers_final_annotation.py:71:1: error[invalid-assignment] Reassignment of `Final` symbol `RATE` is not allowed: Symbol later reassigned here
 qualifiers_final_annotation.py:81:1: error[invalid-assignment] Cannot assign to final attribute `DEFAULT_ID` on type `<class 'ClassB'>`
@@ -839,12 +850,7 @@
 specialtypes_type.py:120:5: error[unresolved-attribute] Type `type` has no attribute `unknown`
 specialtypes_type.py:127:5: error[type-assertion-failure] Argument does not have asserted type `ProUser`
 specialtypes_type.py:137:5: error[type-assertion-failure] Argument does not have asserted type `type[Any]`
-specialtypes_type.py:138:5: error[type-assertion-failure] Argument does not have asserted type `type[Any]`
 specialtypes_type.py:139:5: error[type-assertion-failure] Argument does not have asserted type `type[Any]`
-specialtypes_type.py:140:5: error[type-assertion-failure] Argument does not have asserted type `type[Any]`
-specialtypes_type.py:143:1: error[unresolved-attribute] Type `typing.Type` has no attribute `unknown`
-specialtypes_type.py:145:1: error[unresolved-attribute] Type `<class 'type'>` has no attribute `unknown`
-specialtypes_type.py:146:1: error[unresolved-attribute] Type `GenericAlias` has no attribute `unknown`
 specialtypes_type.py:160:1: error[type-assertion-failure] Argument does not have asserted type `int`
 specialtypes_type.py:161:1: error[type-assertion-failure] Argument does not have asserted type `int`
 specialtypes_type.py:169:5: error[invalid-assignment] Object of type `type` is not assignable to `type[int]`
@@ -862,9 +868,7 @@
 tuples_type_compat.py:106:13: error[type-assertion-failure] Argument does not have asserted type `tuple[str, str] | tuple[int, int]`
 tuples_type_compat.py:111:13: error[type-assertion-failure] Argument does not have asserted type `tuple[int, str, int]`
 tuples_type_compat.py:126:13: error[type-assertion-failure] Argument does not have asserted type `tuple[int | str, str]`
-tuples_type_compat.py:127:13: error[type-assertion-failure] Argument does not have asserted type `@Todo(Support for `typing.TypeAlias`)`
 tuples_type_compat.py:129:13: error[type-assertion-failure] Argument does not have asserted type `tuple[int | str, int]`
-tuples_type_compat.py:130:13: error[type-assertion-failure] Argument does not have asserted type `@Todo(Support for `typing.TypeAlias`)`
 tuples_type_compat.py:153:5: error[type-assertion-failure] Argument does not have asserted type `Sequence[Never]`
 tuples_type_compat.py:157:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[""], Literal[""]]` is not assignable to `tuple[int, str]`
 tuples_type_compat.py:162:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[1], Literal[""]]` is not assignable to `tuple[int, *tuple[str, ...]]`
@@ -898,5 +902,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 900 diagnostics
+Found 904 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-08-27 00:46_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fpep613alias?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #20107 will **improve performances by 21.89%**

<sub>Comparing <code>cjm/pep613alias</code> (79a1305) with <code>main</code> (3db5d59)</sub>



### Summary

`⚡ 1` improvement  
`✅ 12` untouched  
`⏩ 39` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fpep613alias?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 757.2 ms | 621.3 ms | +21.89% |
[^skipped]: 39 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fpep613alias?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @codspeed-hq[bot] on 2025-08-29 02:47_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fpep613alias?runnerMode=Instrumentation)

### Merging #20107 will **degrade performances by 18.89%**

<sub>Comparing <code>cjm/pep613alias</code> (e2ea575) with <code>main</code> (a24a4b5)</sub>



### Summary

`❌ 1` regressions  
`✅ 42` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fpep613alias?runnerMode=Instrumentation)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` hydra-zen `` | 714.9 ms | 881.4 ms | -18.89% |


---

_Comment by @github-actions[bot] on 2025-08-29 20:39_

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

_@AlexWaygood reviewed on 2025-10-16 12:51_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:11788 on 2025-10-16 12:51_

I don't understand why the old assertion no longer passes after rebasing this PR on `main`. (It seemed to pass on earlier versions of this PR, IIRC.) This PR adds new `Type` variants, but the wrapped data is all Salsa-interned, and (from local experimentation -- I tried adding some more `static_assertions::assert_eq_size!()` calls on various wrapped structs and ran `cargo check -p ty_python_semantic --release`) doesn't appear particularly large

---

_@MichaReiser reviewed on 2025-10-16 12:51_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:11788 on 2025-10-16 12:51_

hmm...

---

_@AlexWaygood reviewed on 2025-10-16 12:52_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:11788 on 2025-10-16 12:52_

see https://github.com/astral-sh/ruff/pull/20107#discussion_r2435798449

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-10-16 12:57_

---

_Comment by @github-actions[bot] on 2025-10-16 13:03_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

**Failing projects**:

| Project | Old Status | New Status | Old Return Code | New Return Code |
|---------|------------|------------|-----------------|------------------|
| `hydra-zen` | success | abnormal exit | `1` | `101` |

**Diagnostic changes:**

| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 1,300 | 107 | 177 |
| `type-assertion-failure` | 185 | 256 | 32 |
| `call-non-callable` | 389 | 0 | 0 |
| `no-matching-overload` | 296 | 10 | 0 |
| `possibly-missing-attribute` | 221 | 12 | 63 |
| `unused-ignore-comment` | 17 | 246 | 0 |
| `non-subscriptable` | 212 | 38 | 8 |
| `unsupported-operator` | 60 | 137 | 7 |
| `unresolved-attribute` | 61 | 90 | 52 |
| `invalid-assignment` | 67 | 29 | 20 |
| `invalid-return-type` | 26 | 38 | 25 |
| `invalid-type-form` | 12 | 51 | 1 |
| `not-iterable` | 6 | 9 | 7 |
| `possibly-unresolved-reference` | 0 | 19 | 0 |
| `invalid-base` | 17 | 0 | 0 |
| `invalid-exception-caught` | 16 | 0 | 0 |
| `invalid-parameter-default` | 8 | 0 | 7 |
| `unknown-argument` | 5 | 7 | 0 |
| `invalid-context-manager` | 0 | 0 | 10 |
| `possibly-missing-implicit-call` | 5 | 3 | 0 |
| `index-out-of-bounds` | 2 | 1 | 3 |
| `parameter-already-assigned` | 3 | 0 | 0 |
| `invalid-raise` | 1 | 0 | 0 |
| **Total** | **2,909** | **1,053** | **412** |

**[Full report with detailed diff](https://cjm-pep613alias.ecosystem-663.pages.dev/diff)** ([timing results](https://cjm-pep613alias.ecosystem-663.pages.dev/timing))


---

_@MichaReiser reviewed on 2025-10-16 13:04_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:11788 on 2025-10-16 13:04_

I'm not sure what changed but I remember that @ibraheemdev ran into this as well. 

it seems that `SubclassOfType` is 16 bytes and so is `KnownInstanceType`. But `KnownInstanceType` was 12 bytes before. 

---

_@MichaReiser reviewed on 2025-10-16 13:07_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:7497 on 2025-10-16 13:07_

`TypeAliasType` could be a `salsa::Supertype`

---

_Comment by @AlexWaygood on 2025-10-16 13:11_

hah, I got excited about the Codspeed speedup there for a minute before I remembered that that's the project that now panics in mypy_primer 🤣

---

_@AlexWaygood reviewed on 2025-10-16 13:21_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7497 on 2025-10-16 13:21_

I tried it locally, it doesn't help. `Type` is still 192 bits on a release build, not 128

---

_Comment by @AlexWaygood on 2025-10-16 15:01_

Notes on the ecosystem diagnostics:
- The bokeh ones in `src/bokeh/transform.py` are really weird, need to look into them more
- Lots of true positives from `int()` calls due to the fact that we didn't understand typeshed's type alias it used for `int.__new__` before
- Lots of ddtrace hits due to the fact that we now understand the return type of `sys.exc_info()` properly... the typeshed signature there is accurate (it says this function returns `tuple[type[BaseException] | None, BaseException | None, TracebackType | None]`, but we could possibly special-case this function to model that it will always return `tuple[type[BaseException], BaseException, TracebackType]` if you're inside an `except:` block (this is where the hits are coming from)
- The new dulwich diagnostics in `dulwich/ignore.py` (and I think some of the others in that repo too) are false positives that stem from a combination of https://github.com/astral-sh/ty/issues/1100 and some `@Todo` types that we infer when we see a subscription of an object with an intersection type. Here's a minimal repro on this branch:

   ```py
   from typing import TypeVar

   AnyStr = TypeVar("AnyStr", str, bytes)
	
   def escape(x: AnyStr) -> AnyStr:
       return x
	
   def f(x: bytes):
       if x != b"foo":
           reveal_type(x)  # bytes & ~Literal[b"foo"]
           reveal_type(x[1:])  # @Todo(Subscript expressions on intersections)
           reveal_type(escape(x[1:]))  # str
   ```
- Some diagnostics on `graphql` that look like this which seem like they're technically-speaking true positives (type aliases being used in runtime contexts), but don't have very friendly error messages right now:
   ```
   Invalid class base with type `Unknown | typing.TypeAlias` 
   ```

   Edit: or, _are_ these true positives? I guess typeshed does this kind of thing, e.g. in https://github.com/python/typeshed/blob/11c7821a79a8ab7e1982f3ab506db16f1c4a22a9/stdlib/sys/__init__.pyi#L104... I'm not totally sure whether typeshed is doing the Right Thing there though (hmm, I think it was me who wrote that typeshed type alias 🙈)
- Also some `graphql` diagnostics on e.g. `src/graphql/language/printer.py` that definitely look like false positives, and should be looked into
- We seem to infer the type of `np.float32` as being `object` on this branch? Seems probably incorrect. Is causing _many_ false-positive diagnostics on jax/pandas/sklearn/scipy -- there are many diagnostics like this:
   ```
   Object of type `object` is not callable 
   ```
- This diagnostic on `kornia` requires special-casing by ty because it's true that you can do `isinstance(x, typing.List)` at runtime, but that's not expressible in typeshed's stubs (and typeshed doesn't try to capture it):

   ```
   Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.List`
   ```

   This doesn't really come up that much, especially now that these legacy `typing`-module aliases are all deprecated; we could add that special-casing as a followup. There are some similar diagnostics in `pip` too, though, regarding `typing.Callable`, and in `prefect` regarding `typing.Dict`.
- The win32 errors on `com/win32comext/shell/demos/walk_shell_folders.py` look weird. (Lots of diagnostics like that across the pywin32 project.) Not sure why it thinks that `shell.error` isn't a subclass of `Exception`, since in typeshed's stubs for pywin32 [`error`](https://github.com/python/typeshed/blob/11c7821a79a8ab7e1982f3ab506db16f1c4a22a9/stubs/pywin32/win32comext/shell/shell.pyi#L7) is a type alias for `com_error`, which [definitely inherits from `Exception` in typeshed's stubs](https://github.com/python/typeshed/blob/main/stubs/pywin32/win32/lib/pywintypes.pyi#L17). Maybe this is another example of a type-alias-in-runtime-context issue?
- This one on Sphinx is an issue where we need to use type context to avoid promoting literals when solving the type parameters for a `set` literal (cc. @ibraheemdev):

   ```
   [error] invalid-argument-type - [:177:50] - Argument to bound method `__init__` is incorrect: Expected `AbstractSet[Literal["", "env", "epub", "gettext", "html", "applehelp", "devhelp"]]`, found `frozenset[Unknown | str]`
   ```

---

_Comment by @carljm on 2025-11-20 04:54_

Closing in favor of #21394 

---

_Closed by @carljm on 2025-11-20 04:54_

---
