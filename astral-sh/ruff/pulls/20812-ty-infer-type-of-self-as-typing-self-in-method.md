```yaml
number: 20812
title: "[ty] Infer type of self as typing.Self in method body (with updated salsa, micha playground)"
type: pull_request
state: closed
author: MichaReiser
labels:
  - testing
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: micha/typing-self-function-scope
created_at: 2025-10-11T16:07:22Z
updated_at: 2025-11-16T11:47:05Z
url: https://github.com/astral-sh/ruff/pull/20812
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Infer type of self as typing.Self in method body (with updated salsa, micha playground)

---

_Pull request opened by @MichaReiser on 2025-10-11 16:07_

This is https://github.com/astral-sh/ruff/pull/18473 but my own version where I play with an updated Salsa version.

I don't plan on merging this PR. It's just to get perf numbers, see if mypy primer passes etc.

---

_Label `testing` added by @MichaReiser on 2025-10-11 16:08_

---

_Label `ty` added by @MichaReiser on 2025-10-11 16:08_

---

_Comment by @github-actions[bot] on 2025-10-11 16:09_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-11 16:27:50.031016480 +0000
+++ new-output.txt	2025-10-11 16:27:53.409029431 +0000
@@ -1,6 +1,6 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(cc17)): execute: too many cycle iterations`
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1603f)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/f89c430/src/function/execute.rs:401:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(cc17)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/f89c430/src/function/execute.rs:401:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1643f)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
@@ -346,6 +346,7 @@
 enums_member_names.py:30:5: error[type-assertion-failure] Argument does not have asserted type `Literal["RED", "BLUE", "GREEN"]`
 enums_member_values.py:30:5: error[type-assertion-failure] Argument does not have asserted type `Literal[1, 2, 3]`
 enums_member_values.py:54:1: error[type-assertion-failure] Argument does not have asserted type `Literal[1]`
+enums_member_values.py:85:9: error[invalid-assignment] Object of type `int` is not assignable to attribute `_value_` of type `str`
 enums_member_values.py:96:1: error[type-assertion-failure] Argument does not have asserted type `int`
 enums_members.py:128:21: info[revealed-type] Revealed type: `Unknown | Literal[2]`
 enums_members.py:129:9: error[type-assertion-failure] Argument does not have asserted type `Unknown`
@@ -437,7 +438,6 @@
 generics_self_advanced.py:18:1: error[type-assertion-failure] Argument does not have asserted type `ParentA`
 generics_self_advanced.py:19:1: error[type-assertion-failure] Argument does not have asserted type `ChildA`
 generics_self_advanced.py:28:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@method1`
-generics_self_advanced.py:35:9: error[type-assertion-failure] Argument does not have asserted type `Self@method2`
 generics_self_advanced.py:36:9: error[type-assertion-failure] Argument does not have asserted type `list[Self@method2]`
 generics_self_advanced.py:37:9: error[type-assertion-failure] Argument does not have asserted type `Self@method2`
 generics_self_advanced.py:38:9: error[type-assertion-failure] Argument does not have asserted type `Self@method2`
@@ -447,7 +447,6 @@
 generics_self_attributes.py:26:33: error[invalid-argument-type] Argument is incorrect: Expected `typing.Self | None`, found `LinkedList[int]`
 generics_self_attributes.py:29:5: error[invalid-assignment] Object of type `OrdinalLinkedList` is not assignable to attribute `next` of type `typing.Self | None`
 generics_self_attributes.py:32:5: error[invalid-assignment] Object of type `LinkedList[int]` is not assignable to attribute `next` of type `typing.Self | None`
-generics_self_basic.py:14:9: error[type-assertion-failure] Argument does not have asserted type `Self@set_scale`
 generics_self_basic.py:20:16: error[invalid-return-type] Return type does not match returned value: expected `Self@method2`, found `Shape`
 generics_self_basic.py:33:16: error[invalid-return-type] Return type does not match returned value: expected `Self@cls_method2`, found `Shape`
 generics_self_basic.py:54:1: error[type-assertion-failure] Argument does not have asserted type `Shape`
@@ -491,6 +490,7 @@
 generics_syntax_infer_variance.py:46:1: error[invalid-assignment] Object of type `ShouldBeCovariant3[int | float]` is not assignable to `ShouldBeCovariant3[int]`
 generics_syntax_infer_variance.py:74:1: error[invalid-assignment] Object of type `ShouldBeCovariant5[int]` is not assignable to `ShouldBeCovariant5[int | float]`
 generics_syntax_infer_variance.py:75:1: error[invalid-assignment] Object of type `ShouldBeCovariant5[int | float]` is not assignable to `ShouldBeCovariant5[int]`
+generics_syntax_infer_variance.py:82:9: error[invalid-assignment] Cannot assign to final attribute `x` on type `Self@__init__`
 generics_syntax_infer_variance.py:85:1: error[invalid-assignment] Object of type `ShouldBeCovariant6[int]` is not assignable to `ShouldBeCovariant6[int | float]`
 generics_syntax_infer_variance.py:86:1: error[invalid-assignment] Object of type `ShouldBeCovariant6[int | float]` is not assignable to `ShouldBeCovariant6[int]`
 generics_syntax_infer_variance.py:102:1: error[invalid-assignment] Object of type `ShouldBeInvariant1[int]` is not assignable to `ShouldBeInvariant1[int | float]`
@@ -756,6 +756,8 @@
 protocols_definition.py:287:1: error[invalid-assignment] Object of type `Concrete5_Bad3` is not assignable to `Template5`
 protocols_definition.py:288:1: error[invalid-assignment] Object of type `Concrete5_Bad4` is not assignable to `Template5`
 protocols_definition.py:289:1: error[invalid-assignment] Object of type `Concrete5_Bad5` is not assignable to `Template5`
+protocols_explicit.py:56:9: error[invalid-assignment] Object of type `tuple[int, int, str]` is not assignable to attribute `rgb` of type `tuple[int, int, int]`
+protocols_explicit.py:85:9: error[invalid-attribute-access] Cannot assign to ClassVar `cm1` from an instance of type `Self@__init__`
 protocols_generic.py:40:1: error[invalid-assignment] Object of type `Concrete1` is not assignable to `Proto1[int, str]`
 protocols_generic.py:56:5: error[invalid-assignment] Object of type `Box[int | float]` is not assignable to `Box[int]`
 protocols_generic.py:66:5: error[invalid-assignment] Object of type `Sender[int]` is not assignable to `Sender[int | float]`
@@ -800,6 +802,11 @@
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
@@ -897,5 +904,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 898 diagnostics
+Found 905 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Label `ecosystem-analyzer` added by @MichaReiser on 2025-10-11 16:14_

---

_Comment by @github-actions[bot] on 2025-10-11 16:19_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

**Failing projects**:

| Project | Old Status | New Status | Old Return Code | New Return Code |
|---------|------------|------------|-----------------|------------------|
| `zulip` | success | abnormal exit | `1` | `101` |
| `setuptools` | success | abnormal exit | `1` | `101` |
| `pandas` | success | abnormal exit | `1` | `101` |
| `bokeh` | success | abnormal exit | `1` | `101` |

**Diagnostic changes:**

| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 5,120 | 0 | 0 |
| `possibly-missing-attribute` | 3,148 | 8 | 21 |
| `invalid-argument-type` | 1,725 | 5 | 135 |
| `unused-ignore-comment` | 0 | 1,052 | 0 |
| `unsupported-operator` | 929 | 2 | 7 |
| `invalid-assignment` | 427 | 6 | 15 |
| `invalid-super-argument` | 430 | 4 | 1 |
| `invalid-return-type` | 331 | 64 | 11 |
| `non-subscriptable` | 404 | 0 | 0 |
| `call-non-callable` | 204 | 0 | 0 |
| `missing-argument` | 192 | 0 | 0 |
| `too-many-positional-arguments` | 173 | 0 | 0 |
| `no-matching-overload` | 109 | 0 | 0 |
| `not-iterable` | 105 | 0 | 0 |
| `possibly-missing-implicit-call` | 85 | 4 | 11 |
| `deprecated` | 28 | 0 | 0 |
| `unknown-argument` | 25 | 0 | 0 |
| `parameter-already-assigned` | 17 | 4 | 0 |
| `possibly-unresolved-reference` | 0 | 18 | 0 |
| `redundant-cast` | 13 | 0 | 0 |
| `division-by-zero` | 6 | 4 | 0 |
| `index-out-of-bounds` | 4 | 0 | 0 |
| `invalid-await` | 3 | 0 | 1 |
| `invalid-context-manager` | 3 | 0 | 0 |
| `type-assertion-failure` | 0 | 3 | 0 |
| `invalid-key` | 2 | 0 | 0 |
| `missing-typed-dict-key` | 2 | 0 | 0 |
| `unsupported-base` | 2 | 0 | 0 |
| `invalid-parameter-default` | 1 | 0 | 0 |
| `invalid-type-form` | 1 | 0 | 0 |
| **Total** | **13,489** | **1,174** | **202** |

**[Full report with detailed diff](https://micha-typing-self-function-s.ecosystem-663.pages.dev/diff)** ([timing results](https://micha-typing-self-function-s.ecosystem-663.pages.dev/timing))


---

_Comment by @codspeed-hq[bot] on 2025-10-11 16:20_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Ftyping-self-function-scope)

### Merging #20812 will **degrade performances by 15.96%**

<sub>Comparing <code>micha/typing-self-function-scope</code> (cbc7305) with <code>main</code> (dc64c08)</sub>



### Summary

`❌ 7` regressions  
`✅ 44` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Ftyping-self-function-scope)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Instrumentation | [`` anyio ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ftyping-self-function-scope?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Aanyio%3A%3Aproject%3A%3Aanyio&runnerMode=Instrumentation) | 905.5 ms | 992.8 ms | -8.79% |
| ❌ | WallTime | [`` large[sympy] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ftyping-self-function-scope?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bsympy%5D&runnerMode=WallTime) | 42.6 s | 46.1 s | -7.53% |
| ❌ | WallTime | [`` medium[pandas] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ftyping-self-function-scope?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bpandas%5D&runnerMode=WallTime) | 34.5 s | 39.1 s | -11.84% |
| ❌ | WallTime | [`` medium[static-frame] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ftyping-self-function-scope?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bstatic-frame%5D&runnerMode=WallTime) | 9.5 s | 10.5 s | -9.4% |
| ❌ | WallTime | [`` small[freqtrade] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ftyping-self-function-scope?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Bfreqtrade%5D&runnerMode=WallTime) | 5 s | 5.6 s | -10.95% |
| ❌ | WallTime | [`` small[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ftyping-self-function-scope?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Bpydantic%5D&runnerMode=WallTime) | 2.6 s | 2.8 s | -6.79% |
| ❌ | WallTime | [`` small[tanjun] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ftyping-self-function-scope?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Btanjun%5D&runnerMode=WallTime) | 1.7 s | 2 s | -15.96% |


---

_Comment by @MichaReiser on 2025-10-11 16:31_

Setuptools now panics with too many iterations for `setuptools/_vendor/jaraco/collections/__init__.py`. This seems correct as we now end up with a pretty ridiculous type. The other project also fail because of too many iteration errors. This seems no longer salsa-related but is an issue with our convergence functions.

---

_Closed by @MichaReiser on 2025-10-11 16:39_

---

_Branch deleted on 2025-11-16 11:47_

---
