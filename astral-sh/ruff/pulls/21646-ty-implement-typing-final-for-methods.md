```yaml
number: 21646
title: "[ty] Implement `typing.final` for methods"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/final-method
created_at: 2025-11-26T21:19:41Z
updated_at: 2025-12-03T11:51:20Z
url: https://github.com/astral-sh/ruff/pull/21646
synced_at: 2026-01-12T15:57:30Z
```

# [ty] Implement `typing.final` for methods

---

_@AlexWaygood_

## Summary

A method or property on a superclass decorated with `@final` cannot be overridden on a subclass.

Fixes https://github.com/astral-sh/ty/issues/546.

This currently only implements the check for properties and function-literal types in the class body, not for methods that end up as having other types in the class body (e.g. those decorated with `@lru_cache`). I'll open a followup issue tracking that, if this is merged.

## Test Plan

mdtests/snapshots


---

_Label `ty` added by @AlexWaygood on 2025-11-26 21:19_

---

_Comment by @astral-sh-bot[bot] on 2025-11-26 21:24_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-28 15:14:58.574457152 +0000
+++ new-output.txt	2025-11-28 15:15:02.124465943 +0000
@@ -795,6 +795,7 @@
 overloads_definitions.py:146:48: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | str`
 overloads_definitions.py:159:46: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | str`
 overloads_definitions.py:167:44: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | str`
+overloads_definitions.py:181:9: error[override-of-final-method] Cannot override final member `final_method` from superclass `Base`
 overloads_definitions.py:183:10: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | str`
 overloads_definitions.py:198:9: error[invalid-explicit-override] Method `bad_override` is decorated with `@override` but does not override anything
 overloads_definitions.py:198:45: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | str`
@@ -807,6 +808,7 @@
 overloads_definitions_stub.pyi:44:9: error[invalid-overload] Overloaded function `func6` does not use the `@classmethod` decorator consistently
 overloads_definitions_stub.pyi:76:9: error[invalid-overload] `@final` decorator should be applied only to the first overload
 overloads_definitions_stub.pyi:86:9: error[invalid-overload] `@final` decorator should be applied only to the first overload
+overloads_definitions_stub.pyi:111:9: error[override-of-final-method] Cannot override final member `final_method` from superclass `Base`
 overloads_definitions_stub.pyi:122:9: error[invalid-explicit-override] Method `bad_override` is decorated with `@override` but does not override anything
 overloads_definitions_stub.pyi:147:9: error[invalid-overload] `@override` decorator should be applied only to the first overload
 overloads_evaluation.py:38:1: error[no-matching-overload] No overload of function `example1_1` matches arguments
@@ -894,8 +896,15 @@
 qualifiers_final_annotation.py:155:9: error[invalid-assignment] Reassignment of `Final` symbol `x` is not allowed: Symbol later reassigned here
 qualifiers_final_annotation.py:166:1: error[invalid-assignment] Reassignment of `Final` symbol `TEN` is not allowed: Reassignment of `Final` symbol
 qualifiers_final_decorator.py:21:16: error[subclass-of-final-class] Class `Derived1` cannot inherit from final class `Base1`
+qualifiers_final_decorator.py:56:9: error[override-of-final-method] Cannot override final member `method1` from superclass `Base2`
+qualifiers_final_decorator.py:60:9: error[override-of-final-method] Cannot override final member `method2` from superclass `Base2`
+qualifiers_final_decorator.py:64:9: error[override-of-final-method] Cannot override final member `method3` from superclass `Base2`
+qualifiers_final_decorator.py:75:9: error[override-of-final-method] Cannot override final member `method4` from superclass `Base2`
 qualifiers_final_decorator.py:89:9: error[invalid-overload] `@final` decorator should be applied only to the overload implementation
+qualifiers_final_decorator.py:89:9: error[override-of-final-method] Cannot override final member `method` from superclass `Base3`
+qualifiers_final_decorator.py:102:9: error[override-of-final-method] Cannot override final member `method` from superclass `Base4`
 qualifiers_final_decorator.py:118:9: error[invalid-method-override] Invalid override of method `method`: Definition is incompatible with `Base5_2.method`
+qualifiers_final_decorator.py:118:9: error[override-of-final-method] Cannot override final member `method` from superclass `Base5_2`
 specialtypes_any.py:86:1: error[type-assertion-failure] Type `int` does not match asserted type `int | @Todo(instance attribute on class with dynamic base)`
 specialtypes_never.py:19:22: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Never`
 specialtypes_never.py:32:12: error[invalid-return-type] Return type does not match returned value: expected `int`, found `Literal["whatever works"]`
@@ -1029,4 +1038,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1031 diagnostics
+Found 1040 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-26 21:26_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 500 diagnostics
+ Found 498 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/builders/gettext.py:286:17: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 594 diagnostics
+ Found 593 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 41 diagnostics
+ Found 42 diagnostics

arviz (https://github.com/arviz-devs/arviz)
+ arviz/stats/stats_utils.py:506:9: error[override-of-final-method] Cannot override final member `copy` from superclass `NDFrame`
- Found 814 diagnostics
+ Found 815 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/indexes/multi.py:1757:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 3594 diagnostics
+ Found 3593 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/core/groupby/generic.pyi:208:9: error[override-of-final-method] Cannot override final member `__iter__` from superclass `BaseGroupBy`
+ pandas-stubs/core/groupby/generic.pyi:456:9: error[override-of-final-method] Cannot override final member `__iter__` from superclass `BaseGroupBy`
- Found 6160 diagnostics
+ Found 6162 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/group/valve.py:162:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- homeassistant/components/group/valve.py:182:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ homeassistant/components/zha/device_tracker.py:65:9: error[override-of-final-method] Cannot override final member `device_info` from superclass `ScannerEntity`
- Found 14531 diagnostics
+ Found 14530 diagnostics

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


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     memo metadata = ~7MB
+     memo metadata = ~8MB

trio (https://github.com/python-trio/trio)
-     struct metadata = ~10MB
+     struct metadata = ~11MB
-     memo metadata = ~33MB
+     memo metadata = ~35MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     struct fields = ~20MB
+     struct fields = ~21MB

prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~626MB
+ TOTAL MEMORY USAGE: ~657MB
-     struct metadata = ~42MB
+     struct metadata = ~47MB
-     struct fields = ~44MB
+     struct fields = ~49MB
-     memo metadata = ~138MB
+     memo metadata = ~159MB


```

</details>




---

_Comment by @AlexWaygood on 2025-11-26 21:27_

All the changes to the typing conformance suite look good; the lines where the new diagnostics appear all have `# E` next to them

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-26 21:27_

---

_Comment by @AlexWaygood on 2025-11-26 21:33_

The primer report also all look correct! All of the new diagnostics already have suppression comments for other tools (pylint, pyright or mypy) that we either interpret as blanket `type: ignore`s, don't recognise (because they're not `type: ignore`s, they're something else), or emit on a slightly different line to the other tool

---

_Comment by @astral-sh-bot[bot] on 2025-11-26 21:35_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `override-of-final-method` | 4 | 0 | 0 |
| `unused-ignore-comment` | 0 | 4 | 0 |
| `invalid-argument-type` | 0 | 0 | 2 |
| `unsupported-base` | 1 | 0 | 0 |
| **Total** | **5** | **4** | **2** |

**[Full report with detailed diff](https://alex-final-method.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-final-method.ecosystem-663.pages.dev/timing))




---

_Comment by @codspeed-hq[bot] on 2025-11-26 21:38_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Ffinal-method?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21646 will **degrade performances by 25.39%**

<sub>Comparing <code>alex/final-method</code> (f8eb0d2) with <code>main</code> (c534bfa)</sub>



### Summary

`❌ 3` regressions  
`✅ 19` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Ffinal-method?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Simulation | [`` ty_micro[many_enum_members] ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Ffinal-method?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_many_enum_members%3A%3Aty_micro%5Bmany_enum_members%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 93.4 ms | 125.1 ms | -25.39% |
| ❌ | WallTime | [`` small[altair] ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Ffinal-method?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Baltair%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 4.1 s | 4.4 s | -5.53% |
| ❌ | WallTime | [`` small[tanjun] ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Ffinal-method?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Btanjun%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 2.4 s | 2.5 s | -4.39% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/alex%2Ffinal-method?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Marked ready for review by @AlexWaygood on 2025-11-26 21:44_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-26 21:44_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-26 21:44_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-26 21:44_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-11-26 21:44_

---

_Comment by @AlexWaygood on 2025-11-26 21:45_

> Merging #21646 will **degrade performances by 25.55%**

you hate to see it, _but_ this regression is only manifesting on one (deliberately strange) microbenchmark, not on any of our real-world benchmarks.

---

_Comment by @AlexWaygood on 2025-11-26 21:48_

hmm

```

    union.md - Unions in calls - Bidirectional Type I… (6ad89b6eeb55bea4)

      crates/ty_python_semantic/resources/mdtest/call/union.md:281 unmatched assertion: error: [invalid-argument-type] "Argument to function `f` is incorrect: Expected `T`, found `dict[str, int] & dict[Unknown | str, Unknown | int]`"
      crates/ty_python_semantic/resources/mdtest/call/union.md:281 unexpected error: 7 [invalid-argument-type] "Argument to function `f` is incorrect: Expected `T`, found `dict[Unknown | str, Unknown | int] & dict[str, int]`"

    To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='union.md - Unions in calls - Bidirectional Type I… (6ad89b6eeb55bea4)'
    MDTEST_TEST_FILTER='union.md - Unions in calls - Bidirectional Type I… (6ad89b6eeb55bea4)' cargo test -p ty_python_semantic --test mdtest -- mdtest__call_union
```

I don't know why anything I've done in this PR would change the intersection/union order here. I'm seeing the same failures locally, though, so I guess I can just change the mdtest assertion. cc. @ibraheemdev -- could this be related to https://github.com/astral-sh/ruff/pull/21267 ?

---

_@AlexWaygood reviewed on 2025-11-26 21:49_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/union.md`:280 on 2025-11-26 21:49_

(see https://github.com/astral-sh/ruff/pull/21646#issuecomment-3583310745)

---

_Comment by @ibraheemdev on 2025-11-26 21:51_

I think https://github.com/astral-sh/ruff/pull/21267 just means started seeing more intersection types in mdtests that we were previously hiding, not the source of the problem.

I suppose it is possible that we are creating the intersection types in a non-deterministic way during multi-inference, but I'm not sure how that would be.

---

_Comment by @AlexWaygood on 2025-11-26 21:53_

I think our intersection builder is _generally_ deterministic. There are a _lot_ of intersection types in our ecosystem diagnostics, so if the order of elements in our intersections was nondeterministic, we would constantly get spurious mypy_primer diffs in CI -- but we don't.

---

_Comment by @carljm on 2025-11-26 21:58_

It's deterministic as long as you add the types in a deterministic order; I suspect somewhere we are building an intersection from a non-deterministically-ordered iterator of types. I'm auditing intersection-building call sites to see if I can find it.

---

_Comment by @AlexWaygood on 2025-11-26 22:05_

> It's deterministic as long as you add the types in a deterministic order

yes, that's what I meant

---

_Comment by @AlexWaygood on 2025-11-26 22:37_

> > Merging #21646 will **degrade performances by 25.55%**
> 
> you hate to see it, _but_ this regression is only manifesting on one (deliberately strange) microbenchmark, not on any of our real-world benchmarks.

Well, okay, after fixing some edge-case bugs there are now regressions on other benchmarks too.

---

_Comment by @AlexWaygood on 2025-11-26 23:26_

I tried playing around with this optimisation locally:

<details>

```diff
diff --git a/crates/ty_python_semantic/src/types/liskov.rs b/crates/ty_python_semantic/src/types/liskov.rs
index c377212831..0b8f354970 100644
--- a/crates/ty_python_semantic/src/types/liskov.rs
+++ b/crates/ty_python_semantic/src/types/liskov.rs
@@ -2,6 +2,8 @@
 //!
 //! [Liskov Substitution Principle]: https://en.wikipedia.org/wiki/Liskov_substitution_principle
 
+use std::cell::LazyCell;
+
 use ruff_db::diagnostic::Annotation;
 use rustc_hash::FxHashSet;
 
@@ -9,7 +11,7 @@ use crate::{
     place::Place,
     semantic_index::{place_table, use_def_map},
     types::{
-        ClassBase, ClassLiteral, ClassType, KnownClass, Type,
+        ClassBase, ClassLiteral, ClassType, KnownClass, SpecialFormType, Type,
         class::CodeGeneratorKind,
         context::InferContext,
         diagnostic::{
@@ -56,7 +58,7 @@ fn check_class_declaration<'db>(
     };
 
     let (literal, specialization) = class.class_literal(db);
-    let class_kind = CodeGeneratorKind::from_class(db, literal, specialization);
+    let class_kind = LazyCell::new(|| CodeGeneratorKind::from_class(db, literal, specialization));
 
     let mut subclass_overrides_superclass_declaration = false;
     let mut has_dynamic_superclass = false;
@@ -171,7 +173,7 @@ fn check_class_declaration<'db>(
 
         // Synthesized `__replace__` methods on dataclasses are not checked
         if &member.name == "__replace__"
-            && matches!(class_kind, Some(CodeGeneratorKind::DataclassLike(_)))
+            && matches!(*class_kind, Some(CodeGeneratorKind::DataclassLike(_)))
         {
             continue;
         }
@@ -209,7 +211,14 @@ fn check_class_declaration<'db>(
             {
                 subclass_overrides_superclass_declaration = true;
             }
-        } else if class_kind == Some(CodeGeneratorKind::NamedTuple) {
+        // avoid checking `class_kind` here just to check if it's a `NamedTuple`:
+        // we can get that from the explicit bases directly, and dereferencing
+        // `class_kind` can result in unnecessary memory use due to it being a
+        // `LazyCell` wrapper around a Salsa-cached method.
+        } else if literal
+            .explicit_bases(db)
+            .contains(&Type::SpecialForm(SpecialFormType::NamedTuple))
+        {
             if !KnownClass::NamedTupleFallback
                 .to_instance(db)
                 .member(db, &member.name)
```

</details>

I can't measure any speedup locally, but it's possible it might also reduce memory usage... I could push it and see what happens in CI? It does add a little more complexity to the code.

---

_@MichaReiser reviewed on 2025-11-27 07:11_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/liskov.rs`:125 on 2025-11-27 07:11_

Given that we do a ton of work here to check liskov, would it make sense to skip the work if the relevant rule is disabled? 

---

_@MichaReiser reviewed on 2025-11-27 07:14_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/liskov.rs`:140 on 2025-11-27 07:14_

This call here introduces cross-module query dependencies as you're looking up a function AST node from the superclass (which could be defined anywhere within the project). You might want to move this check into a salsa query

---

_Comment by @MichaReiser on 2025-11-27 07:18_

> I tried playing around with this optimisation locally:

Is there evidence in the perf profiles that this is where we spend the majority of extra time? The function itself is salsa cached and doesn't look that complicated.

---

_@AlexWaygood reviewed on 2025-11-27 11:12_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/liskov.rs`:125 on 2025-11-27 11:12_

that makes sense -- what's the best/cheapest way to do that? Do I want to just copy some of the logic from here?

https://github.com/astral-sh/ruff/blob/761031f729ee43fe39cdb9b04e0133740a77be17/crates/ty_python_semantic/src/types/context.rs#L424-L436

I tried speculatively creating a diagnostic builder before entering the loop, but that did very strange things to `unused-ignore` comments in unrelated mdtests

---

_@AlexWaygood reviewed on 2025-11-27 11:22_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/liskov.rs`:72 on 2025-11-27 11:22_

@MichaReiser, is it safe for a Salsa query to take in a `ScopedSymbolId` as an input, or do I want this to take in a `Name`?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/liskov.rs`:125 on 2025-11-27 11:25_

note that we already don't do any class checks unless we're in a file we should be checking for diagnostics: https://github.com/astral-sh/ruff/blob/761031f729ee43fe39cdb9b04e0133740a77be17/crates/ty_python_semantic/src/types/infer/builder.rs#L573-L576

The Liskov checks are all called from the `check_class_definitions()` hook

---

_@AlexWaygood reviewed on 2025-11-27 11:25_

---

_Comment by @AlexWaygood on 2025-11-27 11:54_

I ran ty on a large codebase locally with `TY_MEMORY_REPORT=full`, to see where the increased memory usage (and the performance hit) might be coming from.

<details>
<summary>Results on this branch</summary>

```
=======SALSA STRUCTS=======
`Definition`                                       metadata=252.58MB fields=353.61MB count=6314532
`ClassLiteral < 'db >::implicit_attribute_inner_::interned_arguments` metadata=259.75MB fields=185.53MB count=4638323
`Type < 'db >::class_member_with_policy_::interned_arguments` metadata=210.21MB fields=180.18MB count=3753792
`IntersectionType`                                 metadata=72.35MB  fields=169.34MB count=1291881
`FunctionType`                                     metadata=42.29MB  fields=163.73MB count=755161
`Type < 'db >::is_redundant_with_::interned_arguments` metadata=264.42MB fields=151.10MB count=4721858
`Type < 'db >::member_lookup_with_policy_::interned_arguments` metadata=161.90MB fields=138.77MB count=2891106
`CallableType`                                     metadata=28.47MB  fields=107.73MB count=508356
`UnionType`                                        metadata=89.66MB  fields=103.68MB count=1601115
`Expression`                                       metadata=200.50MB fields=100.25MB count=4177057
`Type < 'db >::try_call_dunder_get_::interned_arguments` metadata=81.64MB  fields=69.98MB  count=1457830
`StringLiteralType`                                metadata=61.87MB  fields=69.64MB  count=1104846
`File`                                             metadata=17.49MB  fields=61.69MB  count=273280
`OverloadLiteral`                                  metadata=32.93MB  fields=43.44MB  count=588079
`place_by_id::interned_arguments`                  metadata=96.79MB  fields=37.23MB  count=1861283
`Specialization`                                   metadata=30.49MB  fields=32.77MB  count=544435
`Type < 'db >::apply_specialization_::interned_arguments` metadata=62.47MB  fields=26.77MB  count=1115609
`BoundMethodType`                                  metadata=50.61MB  fields=21.69MB  count=903705
`ScopeId`                                          metadata=28.97MB  fields=12.42MB  count=1034607
`ClassLiteral`                                     metadata=8.43MB   fields=11.87MB  count=150568
`TupleType`                                        metadata=7.20MB   fields=11.14MB  count=128610
`ClassLiteral < 'db >::try_mro_::interned_arguments` metadata=27.78MB  fields=7.94MB   count=495984
`GenericContext`                                   metadata=5.15MB   fields=7.39MB   count=91888
`BoundTypeVarInstance`                             metadata=22.58MB  fields=6.45MB   count=403295
`GenericAlias`                                     metadata=22.10MB  fields=6.31MB   count=394583
`FileModule`                                       metadata=2.82MB   fields=4.94MB   count=50425
`FunctionLiteral`                                  metadata=32.93MB  fields=4.70MB   count=588079
`ExpressionWithContext`                            metadata=10.66MB  fields=4.57MB   count=190395
`code_generator_of_class::interned_arguments`      metadata=15.25MB  fields=4.36MB   count=272241
`ModuleLiteralType`                                metadata=11.21MB  fields=4.31MB   count=215490
`ModuleNameIngredient`                             metadata=2.86MB   fields=4.19MB   count=51003
`TypeVarInstance`                                  metadata=4.85MB   fields=4.15MB   count=86523
`ProtocolInterface`                                metadata=1.82MB   fields=3.67MB   count=32513
`TypeVarIdentity`                                  metadata=4.83MB   fields=3.45MB   count=86284
`Unpack`                                           metadata=2.93MB   fields=2.34MB   count=73168
`BytesLiteralType`                                 metadata=0.49MB   fields=2.01MB   count=8761
`EnumLiteralType`                                  metadata=1.97MB   fields=1.40MB   count=35200
`Project`                                          metadata=0.00MB   fields=0.92MB   count=1
`StarImportPlaceholderPredicate`                   metadata=1.19MB   fields=0.85MB   count=42389
`lookup_dunder_new_inner::interned_arguments`      metadata=2.90MB   fields=0.83MB   count=51814
`BoundSuperType`                                   metadata=1.35MB   fields=0.77MB   count=24019
`ClassLiteral < 'db >::fields_::interned_arguments` metadata=1.33MB   fields=0.72MB   count=25670
`PropertyInstanceType`                             metadata=0.96MB   fields=0.55MB   count=17089
`PatternPredicate`                                 metadata=0.10MB   fields=0.26MB   count=3194
`is_equivalent_to_object_inner::interned_arguments` metadata=0.75MB   fields=0.17MB   count=14356
`FieldInstance`                                    metadata=0.16MB   fields=0.15MB   count=2893
`UnionTypeInstance`                                metadata=0.05MB   fields=0.07MB   count=910
`NewType`                                          metadata=0.06MB   fields=0.05MB   count=1092
`Program`                                          metadata=0.00MB   fields=0.02MB   count=1
`TypeIsType`                                       metadata=0.02MB   fields=0.01MB   count=388
`InternedType`                                     metadata=0.02MB   fields=0.00MB   count=277
`ClassLiteral < 'db >::variance_of_::interned_arguments` metadata=0.01MB   fields=0.00MB   count=127
`DataclassParams`                                  metadata=0.00MB   fields=0.00MB   count=28
`DeprecatedInstance`                               metadata=0.01MB   fields=0.00MB   count=119
`GenericAlias < 'db >::variance_of_::interned_arguments` metadata=0.00MB   fields=0.00MB   count=45
`FileRoot`                                         metadata=0.00MB   fields=0.00MB   count=3
`DataclassTransformerParams`                       metadata=0.00MB   fields=0.00MB   count=5
`NamespacePackage`                                 metadata=0.00MB   fields=0.00MB   count=8
`is_function_definition::interned_arguments`       metadata=0.00MB   fields=0.00MB   count=4
`ModuleResolveModeIngredient`                      metadata=0.00MB   fields=0.00MB   count=1
`module_type_symbols::interned_arguments`          metadata=0.00MB   fields=0.00MB   count=1
=======SALSA QUERIES=======
`semantic_index -> ty_python_semantic::semantic_index::SemanticIndex<'_>`
    metadata=287.40MB fields=6808.31MB count=93616
`parsed_module -> ruff_db::parsed::ParsedModule`
    metadata=7.15MB   fields=3328.33MB count=94119
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference<'_>`
    metadata=1945.58MB fields=1327.92MB count=5351152
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference<'_>`
    metadata=732.23MB fields=1284.14MB count=914076
`infer_expression_types_impl -> ty_python_semantic::types::infer::ExpressionInference<'_>`
    metadata=2050.22MB fields=823.69MB count=3815077
`source_text -> ruff_db::source::SourceText`
    metadata=6.02MB   fields=771.74MB count=94119
`check_file_impl -> core::result::Result<alloc::boxed::Box<[ruff_db::diagnostic::Diagnostic]>, ruff_db::diagnostic::Diagnostic>`
    metadata=21.84MB  fields=199.91MB count=90643
`ClassLiteral < 'db >::implicit_attribute_inner_ -> ty_python_semantic::types::member::Member<'_>`
    metadata=516.93MB fields=148.43MB count=4638323
`FunctionType < 'db >::signature_ -> ty_python_semantic::types::signatures::CallableSignature<'_>`
    metadata=94.35MB  fields=147.94MB count=628226
`Type < 'db >::class_member_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=1078.42MB fields=120.12MB count=3753792
`Type < 'db >::member_lookup_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=1071.66MB fields=92.52MB  count=2891106
`ClassLiteral < 'db >::try_mro_ -> core::result::Result<ty_python_semantic::types::mro::Mro<'_>, ty_python_semantic::types::mro::MroError<'_>>`
    metadata=113.55MB fields=72.07MB  count=495984
`FunctionType < 'db >::last_definition_raw_signature_ -> ty_python_semantic::types::signatures::Signature<'_>`
    metadata=43.12MB  fields=63.79MB  count=291911
`place_by_id -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=155.28MB fields=59.56MB  count=1861283
`line_index -> ruff_source_file::line_index::LineIndex`
    metadata=1.14MB   fields=56.57MB  count=21920
`all_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type<'_>, rustc_hash::FxBuildHasher>>`
    metadata=214.31MB fields=43.13MB  count=528646
`Type < 'db >::try_call_dunder_get_ -> core::option::Option<(ty_python_semantic::types::Type<'_>, ty_python_semantic::types::AttributeKind)>`
    metadata=764.56MB fields=34.99MB  count=1457830
`infer_deferred_types -> ty_python_semantic::types::infer::DefinitionInference<'_>`
    metadata=37.43MB  fields=28.17MB  count=107086
`all_negative_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type<'_>, rustc_hash::FxBuildHasher>>`
    metadata=46.84MB  fields=25.14MB  count=305015
`ClassLiteral < 'db >::fields_ -> indexmap::map::IndexMap<ruff_python_ast::name::Name, ty_python_semantic::types::class::Field<'_>, core::hash::BuildHasherDefault<rustc_hash::FxHasher>>`
    metadata=17.94MB  fields=18.59MB  count=25670
`Type < 'db >::apply_specialization_ -> ty_python_semantic::types::Type<'_>`
    metadata=175.84MB fields=17.85MB  count=1115609
`infer_expression_type_impl -> ty_python_semantic::types::Type<'_>`
    metadata=84.91MB  fields=16.10MB  count=1006467
`suppressions -> ty_python_semantic::suppression::Suppressions`
    metadata=5.80MB   fields=14.23MB  count=90643
`FunctionLiteral < 'db >::overloads_and_implementation_ -> (alloc::boxed::Box<[ty_python_semantic::types::function::OverloadLiteral<'_>]>, core::option::Option<ty_python_semantic::types::function::OverloadLiteral<'_>>)`
    metadata=42.37MB  fields=14.17MB  count=586429
`enum_metadata -> core::option::Option<ty_python_semantic::types::enums::EnumMetadata<'_>>`
    metadata=62.22MB  fields=12.67MB  count=143963
`FunctionType < 'db >::last_definition_signature_ -> ty_python_semantic::types::signatures::Signature<'_>`
    metadata=10.29MB  fields=12.57MB  count=51169
`infer_unpack_types -> ty_python_semantic::types::unpacker::UnpackResult<'_>`
    metadata=19.62MB  fields=11.86MB  count=65431
`ClassLiteral < 'db >::try_metaclass_ -> core::result::Result<(ty_python_semantic::types::Type<'_>, core::option::Option<ty_python_semantic::types::function::DataclassTransformerParams<'_>>), ty_python_semantic::types::class::MetaclassError<'_>>`
    metadata=31.98MB  fields=7.18MB   count=149545
`imported_modules -> alloc::sync::Arc<std::collections::hash::set::HashSet<ty_python_semantic::module_name::ModuleName, rustc_hash::FxBuildHasher>>`
    metadata=2.85MB   fields=6.61MB   count=54886
`inferable_typevars_inner -> std::collections::hash::set::HashSet<ty_python_semantic::types::BoundTypeVarIdentity<'_>, rustc_hash::FxBuildHasher>`
    metadata=3.13MB   fields=5.21MB   count=63330
`Type < 'db >::is_redundant_with_ -> bool`
    metadata=498.28MB fields=4.72MB   count=4721858
`ClassLiteral < 'db >::explicit_bases_ -> alloc::boxed::Box<[ty_python_semantic::types::Type<'_>]>`
    metadata=12.46MB  fields=4.68MB   count=150348
`code_generator_of_class -> core::option::Option<ty_python_semantic::types::class::CodeGeneratorKind<'_>>`
    metadata=35.62MB  fields=3.27MB   count=272241
`place_table -> alloc::sync::Arc<ty_python_semantic::semantic_index::place::PlaceTable>`
    metadata=20.07MB  fields=3.09MB   count=385988
`BoundMethodType < 'db >::into_callable_type_ -> ty_python_semantic::types::CallableType<'_>`
    metadata=45.34MB  fields=2.79MB   count=349181
`ClassLiteral < 'db >::decorators_ -> alloc::boxed::Box<[ty_python_semantic::types::Type<'_>]>`
    metadata=10.80MB  fields=2.79MB   count=145217
`use_def_map -> alloc::sync::Arc<ty_python_semantic::semantic_index::use_def::UseDefMap<'_>>`
    metadata=11.48MB  fields=1.77MB   count=220792
`lookup_dunder_new_inner -> core::option::Option<ty_python_semantic::place::PlaceAndQualifiers<'_>>`
    metadata=28.70MB  fields=1.66MB   count=51814
`ClassLiteral < 'db >::pep695_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=12.03MB  fields=1.20MB   count=150347
`ClassLiteral < 'db >::inherited_legacy_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=12.01MB  fields=1.20MB   count=150052
`exported_names -> alloc::boxed::Box<[ruff_python_ast::name::Name]>`
    metadata=0.08MB   fields=1.12MB   count=915
`file_settings -> ty_project::metadata::settings::FileSettings`
    metadata=6.53MB   fields=0.73MB   count=90643
`static_expression_truthiness -> ty_python_semantic::types::Truthiness`
    metadata=63.00MB  fields=0.66MB   count=660157
`resolve_module_query -> core::option::Option<ty_python_semantic::module_resolver::module::Module<'_>>`
    metadata=22.04MB  fields=0.61MB   count=51003
`global_scope -> ty_python_semantic::semantic_index::scope::ScopeId<'_>`
    metadata=3.76MB   fields=0.58MB   count=72265
`TupleType < 'db >::to_class_type_ -> ty_python_semantic::types::class::ClassType<'_>`
    metadata=12.87MB  fields=0.23MB   count=19054
`cached_protocol_interface -> ty_python_semantic::types::protocol_class::ProtocolInterface<'_>`
    metadata=8.37MB   fields=0.23MB   count=28410
`all_narrowing_constraints_for_pattern -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type<'_>, rustc_hash::FxBuildHasher>>`
    metadata=17.60MB  fields=0.19MB   count=2285
`all_negative_narrowing_constraints_for_pattern -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type<'_>, rustc_hash::FxBuildHasher>>`
    metadata=0.40MB   fields=0.19MB   count=2115
`ClassLiteral < 'db >::is_typed_dict_ -> bool`
    metadata=17.30MB  fields=0.15MB   count=150123
`ClassLiteral < 'db >::inheritance_cycle_ -> core::option::Option<ty_python_semantic::types::class::InheritanceCycle>`
    metadata=17.76MB  fields=0.15MB   count=148297
`dunder_all_names -> core::option::Option<std::collections::hash::set::HashSet<ruff_python_ast::name::Name, rustc_hash::FxBuildHasher>>`
    metadata=0.04MB   fields=0.08MB   count=360
`file_to_module -> core::option::Option<ty_python_semantic::module_resolver::module::Module<'_>>`
    metadata=0.35MB   fields=0.04MB   count=3212
`ClassType < 'db >::into_callable_ -> ty_python_semantic::types::CallableTypes<'_>`
    metadata=0.16MB   fields=0.01MB   count=613
`is_equivalent_to_object_inner -> bool`
    metadata=4.38MB   fields=0.01MB   count=14356
`TypeVarInstance < 'db >::lazy_bound_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints<'_>>`
    metadata=0.09MB   fields=0.01MB   count=852
`NewType < 'db >::lazy_base_ -> ty_python_semantic::types::newtype::NewTypeBase<'_>`
    metadata=0.18MB   fields=0.01MB   count=771
`TypeVarInstance < 'db >::lazy_constraints_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints<'_>>`
    metadata=0.01MB   fields=0.00MB   count=57
`module_type_symbols -> smallvec::SmallVec<[ruff_python_ast::name::Name; 8]>`
    metadata=0.00MB   fields=0.00MB   count=1
`TypeVarInstance < 'db >::lazy_default_ -> core::option::Option<ty_python_semantic::types::Type<'_>>`
    metadata=0.00MB   fields=0.00MB   count=29
`ClassLiteral < 'db >::variance_of_ -> ty_python_semantic::types::variance::TypeVarVariance`
    metadata=0.02MB   fields=0.00MB   count=127
`dynamic_resolution_paths -> alloc::vec::Vec<ty_python_semantic::module_resolver::path::SearchPath>`
    metadata=0.00MB   fields=0.00MB   count=1
`GenericAlias < 'db >::variance_of_ -> ty_python_semantic::types::variance::TypeVarVariance`
    metadata=0.00MB   fields=0.00MB   count=45
`is_function_definition -> bool`
    metadata=0.00MB   fields=0.00MB   count=4
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 30482.65MB
    struct metadata = 2240.14MB
    struct fields = 2130.14MB
    memo metadata = 10506.70MB
    memo fields = 15605.67MB
```

</details>

<details>
<summary>Results on main</summary>

```
=======SALSA STRUCTS=======
`Definition`                                       metadata=252.58MB fields=353.61MB count=6314449
`FunctionType`                                     metadata=42.24MB  fields=163.24MB count=754355
`IntersectionType`                                 metadata=67.55MB  fields=158.38MB count=1206228
`Type < 'db >::class_member_with_policy_::interned_arguments` metadata=184.12MB fields=157.81MB count=3287789
`Type < 'db >::is_redundant_with_::interned_arguments` metadata=250.83MB fields=143.33MB count=4479056
`ClassLiteral < 'db >::implicit_attribute_inner_::interned_arguments` metadata=189.96MB fields=135.68MB count=3392059
`Type < 'db >::member_lookup_with_policy_::interned_arguments` metadata=140.53MB fields=120.46MB count=2509480
`CallableType`                                     metadata=28.46MB  fields=107.72MB count=508295
`Expression`                                       metadata=200.50MB fields=100.25MB count=4177018
`UnionType`                                        metadata=83.39MB  fields=97.58MB  count=1489117
`StringLiteralType`                                metadata=61.86MB  fields=69.63MB  count=1104613
`File`                                             metadata=17.49MB  fields=61.69MB  count=273272
`Type < 'db >::try_call_dunder_get_::interned_arguments` metadata=67.38MB  fields=57.75MB  count=1203194
`OverloadLiteral`                                  metadata=32.93MB  fields=43.44MB  count=587994
`place_by_id::interned_arguments`                  metadata=82.91MB  fields=31.89MB  count=1594423
`Specialization`                                   metadata=29.00MB  fields=31.21MB  count=517813
`Type < 'db >::apply_specialization_::interned_arguments` metadata=58.88MB  fields=25.23MB  count=1051346
`BoundMethodType`                                  metadata=50.08MB  fields=21.46MB  count=894229
`ScopeId`                                          metadata=28.97MB  fields=12.42MB  count=1034589
`ClassLiteral`                                     metadata=8.43MB   fields=11.87MB  count=150562
`TupleType`                                        metadata=7.20MB   fields=11.13MB  count=128600
`ClassLiteral < 'db >::try_mro_::interned_arguments` metadata=26.63MB  fields=7.61MB   count=475488
`GenericContext`                                   metadata=5.14MB   fields=7.38MB   count=91738
`BoundTypeVarInstance`                             metadata=22.58MB  fields=6.45MB   count=403283
`GenericAlias`                                     metadata=21.29MB  fields=6.08MB   count=380135
`FileModule`                                       metadata=2.82MB   fields=4.94MB   count=50423
`FunctionLiteral`                                  metadata=32.93MB  fields=4.70MB   count=587994
`ExpressionWithContext`                            metadata=10.66MB  fields=4.57MB   count=190373
`ModuleLiteralType`                                metadata=11.21MB  fields=4.31MB   count=215485
`ModuleNameIngredient`                             metadata=2.86MB   fields=4.19MB   count=51001
`TypeVarInstance`                                  metadata=4.85MB   fields=4.15MB   count=86519
`code_generator_of_class::interned_arguments`      metadata=14.00MB  fields=4.00MB   count=250050
`ProtocolInterface`                                metadata=1.82MB   fields=3.66MB   count=32509
`TypeVarIdentity`                                  metadata=4.83MB   fields=3.45MB   count=86280
`Unpack`                                           metadata=2.93MB   fields=2.34MB   count=73167
`BytesLiteralType`                                 metadata=0.49MB   fields=2.01MB   count=8761
`EnumLiteralType`                                  metadata=1.97MB   fields=1.40MB   count=35198
`Project`                                          metadata=0.00MB   fields=0.92MB   count=1
`StarImportPlaceholderPredicate`                   metadata=1.19MB   fields=0.85MB   count=42389
`lookup_dunder_new_inner::interned_arguments`      metadata=2.90MB   fields=0.83MB   count=51812
`BoundSuperType`                                   metadata=1.35MB   fields=0.77MB   count=24019
`ClassLiteral < 'db >::fields_::interned_arguments` metadata=1.33MB   fields=0.72MB   count=25670
`PropertyInstanceType`                             metadata=0.95MB   fields=0.54MB   count=17005
`PatternPredicate`                                 metadata=0.10MB   fields=0.26MB   count=3194
`is_equivalent_to_object_inner::interned_arguments` metadata=0.75MB   fields=0.17MB   count=14349
`FieldInstance`                                    metadata=0.16MB   fields=0.15MB   count=2893
`UnionTypeInstance`                                metadata=0.05MB   fields=0.07MB   count=910
`NewType`                                          metadata=0.06MB   fields=0.05MB   count=1061
`Program`                                          metadata=0.00MB   fields=0.02MB   count=1
`TypeIsType`                                       metadata=0.02MB   fields=0.01MB   count=388
`InternedType`                                     metadata=0.02MB   fields=0.00MB   count=277
`ClassLiteral < 'db >::variance_of_::interned_arguments` metadata=0.01MB   fields=0.00MB   count=127
`DataclassParams`                                  metadata=0.00MB   fields=0.00MB   count=28
`DeprecatedInstance`                               metadata=0.01MB   fields=0.00MB   count=118
`GenericAlias < 'db >::variance_of_::interned_arguments` metadata=0.00MB   fields=0.00MB   count=45
`FileRoot`                                         metadata=0.00MB   fields=0.00MB   count=3
`DataclassTransformerParams`                       metadata=0.00MB   fields=0.00MB   count=5
`NamespacePackage`                                 metadata=0.00MB   fields=0.00MB   count=8
`ModuleResolveModeIngredient`                      metadata=0.00MB   fields=0.00MB   count=1
`module_type_symbols::interned_arguments`          metadata=0.00MB   fields=0.00MB   count=1
=======SALSA QUERIES=======
`semantic_index -> ty_python_semantic::semantic_index::SemanticIndex<'_>`
    metadata=287.40MB fields=6808.23MB count=93614
`parsed_module -> ruff_db::parsed::ParsedModule`
    metadata=7.15MB   fields=3356.48MB count=94117
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference<'_>`
    metadata=1945.54MB fields=1327.88MB count=5350978
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference<'_>`
    metadata=707.11MB fields=1284.14MB count=914075
`infer_expression_types_impl -> ty_python_semantic::types::infer::ExpressionInference<'_>`
    metadata=2050.13MB fields=823.67MB count=3815035
`source_text -> ruff_db::source::SourceText`
    metadata=6.02MB   fields=771.74MB count=94117
`check_file_impl -> core::result::Result<alloc::boxed::Box<[ruff_db::diagnostic::Diagnostic]>, ruff_db::diagnostic::Diagnostic>`
    metadata=21.84MB  fields=199.90MB count=90643
`FunctionType < 'db >::signature_ -> ty_python_semantic::types::signatures::CallableSignature<'_>`
    metadata=94.30MB  fields=147.62MB count=627782
`ClassLiteral < 'db >::implicit_attribute_inner_ -> ty_python_semantic::types::member::Member<'_>`
    metadata=396.92MB fields=108.55MB count=3392059
`Type < 'db >::class_member_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=976.98MB fields=105.21MB count=3287789
`Type < 'db >::member_lookup_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=887.72MB fields=80.30MB  count=2509480
`ClassLiteral < 'db >::try_mro_ -> core::result::Result<ty_python_semantic::types::mro::Mro<'_>, ty_python_semantic::types::mro::MroError<'_>>`
    metadata=108.56MB fields=69.22MB  count=475488
`FunctionType < 'db >::last_definition_raw_signature_ -> ty_python_semantic::types::signatures::Signature<'_>`
    metadata=43.12MB  fields=63.79MB  count=291911
`line_index -> ruff_source_file::line_index::LineIndex`
    metadata=1.14MB   fields=56.56MB  count=21917
`place_by_id -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=122.69MB fields=51.02MB  count=1594423
`all_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type<'_>, rustc_hash::FxBuildHasher>>`
    metadata=214.29MB fields=43.12MB  count=528642
`Type < 'db >::try_call_dunder_get_ -> core::option::Option<(ty_python_semantic::types::Type<'_>, ty_python_semantic::types::AttributeKind)>`
    metadata=721.23MB fields=28.88MB  count=1203194
`infer_deferred_types -> ty_python_semantic::types::infer::DefinitionInference<'_>`
    metadata=37.42MB  fields=28.17MB  count=107084
`all_negative_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type<'_>, rustc_hash::FxBuildHasher>>`
    metadata=46.83MB  fields=25.13MB  count=305013
`ClassLiteral < 'db >::fields_ -> indexmap::map::IndexMap<ruff_python_ast::name::Name, ty_python_semantic::types::class::Field<'_>, core::hash::BuildHasherDefault<rustc_hash::FxHasher>>`
    metadata=17.94MB  fields=18.59MB  count=25670
`Type < 'db >::apply_specialization_ -> ty_python_semantic::types::Type<'_>`
    metadata=172.50MB fields=16.82MB  count=1051346
`infer_expression_type_impl -> ty_python_semantic::types::Type<'_>`
    metadata=84.71MB  fields=16.07MB  count=1004549
`suppressions -> ty_python_semantic::suppression::Suppressions`
    metadata=5.80MB   fields=14.23MB  count=90643
`FunctionLiteral < 'db >::overloads_and_implementation_ -> (alloc::boxed::Box<[ty_python_semantic::types::function::OverloadLiteral<'_>]>, core::option::Option<ty_python_semantic::types::function::OverloadLiteral<'_>>)`
    metadata=42.36MB  fields=14.16MB  count=586343
`FunctionType < 'db >::last_definition_signature_ -> ty_python_semantic::types::signatures::Signature<'_>`
    metadata=10.27MB  fields=12.51MB  count=50992
`infer_unpack_types -> ty_python_semantic::types::unpacker::UnpackResult<'_>`
    metadata=19.62MB  fields=11.85MB  count=65429
`enum_metadata -> core::option::Option<ty_python_semantic::types::enums::EnumMetadata<'_>>`
    metadata=55.41MB  fields=11.28MB  count=128187
`ClassLiteral < 'db >::try_metaclass_ -> core::result::Result<(ty_python_semantic::types::Type<'_>, core::option::Option<ty_python_semantic::types::function::DataclassTransformerParams<'_>>), ty_python_semantic::types::class::MetaclassError<'_>>`
    metadata=31.98MB  fields=7.18MB   count=149532
`imported_modules -> alloc::sync::Arc<std::collections::hash::set::HashSet<ty_python_semantic::module_name::ModuleName, rustc_hash::FxBuildHasher>>`
    metadata=2.85MB   fields=6.61MB   count=54884
`inferable_typevars_inner -> std::collections::hash::set::HashSet<ty_python_semantic::types::BoundTypeVarIdentity<'_>, rustc_hash::FxBuildHasher>`
    metadata=3.13MB   fields=5.21MB   count=63323
`ClassLiteral < 'db >::explicit_bases_ -> alloc::boxed::Box<[ty_python_semantic::types::Type<'_>]>`
    metadata=12.46MB  fields=4.68MB   count=150342
`Type < 'db >::is_redundant_with_ -> bool`
    metadata=481.42MB fields=4.48MB   count=4479056
`place_table -> alloc::sync::Arc<ty_python_semantic::semantic_index::place::PlaceTable>`
    metadata=20.07MB  fields=3.09MB   count=385979
`code_generator_of_class -> core::option::Option<ty_python_semantic::types::class::CodeGeneratorKind<'_>>`
    metadata=32.50MB  fields=3.00MB   count=250050
`BoundMethodType < 'db >::into_callable_type_ -> ty_python_semantic::types::CallableType<'_>`
    metadata=45.34MB  fields=2.79MB   count=349186
`ClassLiteral < 'db >::decorators_ -> alloc::boxed::Box<[ty_python_semantic::types::Type<'_>]>`
    metadata=9.16MB   fields=2.40MB   count=122737
`use_def_map -> alloc::sync::Arc<ty_python_semantic::semantic_index::use_def::UseDefMap<'_>>`
    metadata=11.48MB  fields=1.77MB   count=220784
`lookup_dunder_new_inner -> core::option::Option<ty_python_semantic::place::PlaceAndQualifiers<'_>>`
    metadata=28.69MB  fields=1.66MB   count=51812
`ClassLiteral < 'db >::pep695_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=12.03MB  fields=1.20MB   count=150341
`ClassLiteral < 'db >::inherited_legacy_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=12.01MB  fields=1.20MB   count=150046
`exported_names -> alloc::boxed::Box<[ruff_python_ast::name::Name]>`
    metadata=0.08MB   fields=1.12MB   count=915
`file_settings -> ty_project::metadata::settings::FileSettings`
    metadata=6.53MB   fields=0.73MB   count=90643
`static_expression_truthiness -> ty_python_semantic::types::Truthiness`
    metadata=62.97MB  fields=0.66MB   count=660148
`resolve_module_query -> core::option::Option<ty_python_semantic::module_resolver::module::Module<'_>>`
    metadata=22.03MB  fields=0.61MB   count=51001
`global_scope -> ty_python_semantic::semantic_index::scope::ScopeId<'_>`
    metadata=3.76MB   fields=0.58MB   count=72263
`cached_protocol_interface -> ty_python_semantic::types::protocol_class::ProtocolInterface<'_>`
    metadata=8.37MB   fields=0.23MB   count=28410
`TupleType < 'db >::to_class_type_ -> ty_python_semantic::types::class::ClassType<'_>`
    metadata=12.31MB  fields=0.21MB   count=17815
`all_narrowing_constraints_for_pattern -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type<'_>, rustc_hash::FxBuildHasher>>`
    metadata=17.60MB  fields=0.19MB   count=2285
`all_negative_narrowing_constraints_for_pattern -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type<'_>, rustc_hash::FxBuildHasher>>`
    metadata=0.40MB   fields=0.19MB   count=2115
`ClassLiteral < 'db >::is_typed_dict_ -> bool`
    metadata=17.30MB  fields=0.15MB   count=150121
`ClassLiteral < 'db >::inheritance_cycle_ -> core::option::Option<ty_python_semantic::types::class::InheritanceCycle>`
    metadata=17.76MB  fields=0.15MB   count=148291
`dunder_all_names -> core::option::Option<std::collections::hash::set::HashSet<ruff_python_ast::name::Name, rustc_hash::FxBuildHasher>>`
    metadata=0.04MB   fields=0.08MB   count=360
`file_to_module -> core::option::Option<ty_python_semantic::module_resolver::module::Module<'_>>`
    metadata=0.34MB   fields=0.04MB   count=3211
`ClassType < 'db >::into_callable_ -> ty_python_semantic::types::CallableTypes<'_>`
    metadata=0.16MB   fields=0.01MB   count=611
`is_equivalent_to_object_inner -> bool`
    metadata=4.38MB   fields=0.01MB   count=14349
`TypeVarInstance < 'db >::lazy_bound_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints<'_>>`
    metadata=0.09MB   fields=0.01MB   count=847
`NewType < 'db >::lazy_base_ -> ty_python_semantic::types::newtype::NewTypeBase<'_>`
    metadata=0.18MB   fields=0.01MB   count=770
`TypeVarInstance < 'db >::lazy_constraints_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints<'_>>`
    metadata=0.01MB   fields=0.00MB   count=57
`module_type_symbols -> smallvec::SmallVec<[ruff_python_ast::name::Name; 8]>`
    metadata=0.00MB   fields=0.00MB   count=1
`TypeVarInstance < 'db >::lazy_default_ -> core::option::Option<ty_python_semantic::types::Type<'_>>`
    metadata=0.00MB   fields=0.00MB   count=29
`ClassLiteral < 'db >::variance_of_ -> ty_python_semantic::types::variance::TypeVarVariance`
    metadata=0.02MB   fields=0.00MB   count=127
`dynamic_resolution_paths -> alloc::vec::Vec<ty_python_semantic::module_resolver::path::SearchPath>`
    metadata=0.00MB   fields=0.00MB   count=1
`GenericAlias < 'db >::variance_of_ -> ty_python_semantic::types::variance::TypeVarVariance`
    metadata=0.00MB   fields=0.00MB   count=45
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 29561.45MB
    struct metadata = 2061.16MB
    struct fields = 1992.43MB
    memo metadata = 9962.47MB
    memo fields = 15545.40MB
```

</details>

---

_Comment by @AlexWaygood on 2025-11-27 12:16_

The memory reports indicate that the increased memory usage is because we're doing a lot more `Type::member()` calls:

```diff
- `ClassLiteral < 'db >::implicit_attribute_inner_::interned_arguments` metadata=189.96MB fields=135.68MB count=3392059
+ `ClassLiteral < 'db >::implicit_attribute_inner_::interned_arguments` metadata=259.75MB fields=185.53MB count=4638323
- `Type < 'db >::class_member_with_policy_::interned_arguments` metadata=184.12MB fields=157.81MB count=3287789
+ `Type < 'db >::class_member_with_policy_::interned_arguments` metadata=210.21MB fields=180.18MB count=3753792
  `ClassLiteral < 'db >::implicit_attribute_inner_ -> ty_python_semantic::types::member::Member<'_>`
-     metadata=396.92MB fields=108.55MB count=3392059
+     metadata=516.93MB fields=148.43MB count=4638323
  `Type < 'db >::class_member_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
-     metadata=976.98MB fields=105.21MB count=3287789
+     metadata=1078.42MB fields=120.12MB count=3753792
  `Type < 'db >::member_lookup_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
-     metadata=887.72MB fields=80.30MB  count=2509480
+     metadata=1071.66MB fields=92.52MB  count=2891106
```

That makes sense: previously, we only did the `Type::member()` call on the superclass if the definition in the subclass was a function definition, because we currently only do Liskov checks if the subclass and the superclass are both function definitions (this will change in https://github.com/astral-sh/ruff/pull/21611). Now, we have to do the `Type::member()` call for all definitions on the subclass, because any definition on the subclass could possibly override a `@final` method on a superclass. I strongly suspect that the performance hit is caused by the same thing.

I'm not sure there's realistically any way to get around this. With the checks we have in place _right now_ we don't really need to look up implicit instance attributes: we can just look at the type as declared/defined in the class body. So it's _possible_ I could avoid the `Type::member()` call in favour of more low-level APIs that would avoid looking up the member as an implicit instance attributes. But we will continue adding more Liskov checks in the (near) future, which will definitely need to know the types of implicit instance attributes on both the subclass and the superclass. So I don't think it's worth trying to micro-optimise away the `Type::member()` calls for gains that would be very short-term.

---

_@MichaReiser reviewed on 2025-11-27 12:31_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/liskov.rs`:125 on 2025-11-27 12:31_

That was actually the first thing I checked this morning, whether we always run this check or only for first-party files.

I think just `ctx.db.rule_selection(ctx.file).is_enabled(lint)` should be enough


---

_@MichaReiser reviewed on 2025-11-27 12:32_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/liskov.rs`:72 on 2025-11-27 12:32_

I don't see any issue with it, other than that it requires interning (but that's just the cost we have to pay?)

---

_@AlexWaygood reviewed on 2025-11-27 12:38_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/liskov.rs`:72 on 2025-11-27 12:38_

cool, thank you!

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-11-27 13:00_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/final.md`:77 on 2025-11-28 07:33_

I'm getting you're not a fan of whitespace :D
```suggestion
    @final
    def foo(self): ...

    @final
    @property
    def my_property1(self) -> int: ...

    @property
    @final
    def my_property2(self) -> int: ...

    @final
    @classmethod
    def class_method1(cls) -> int: ...

    @staticmethod
    @final
    def static_method1() -> int: ...

    @final
    @classmethod
    def class_method2(cls) -> int: ...

    @staticmethod
    @final
    def static_method2() -> int: ...

    @lossy_decorator
    @final
    def decorated_1(self): ...
    
    @final
    @lossy_decorator
    def decorated_2(self): ...

```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/final.md`:59 on 2025-11-28 07:35_

What's the difference between `class_method1` and `2` tha makes it worth having both?

The same for `static_method1` and `2`. Did you mean to change the decorator ordering?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/final.md`:206 on 2025-11-28 07:40_

What's the reasoning for only emitting one error here rather than one per overload implementation? It seems a bit annoying that, after removing the last overload, more diagnostics will pop up

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/final.md`:271 on 2025-11-28 07:44_

I find the diagnostic range here confusing. I read the message like 4 times and was confused why ty would think that the definition of `f` on line 248 isn't the implementation when it clearly is. I think we should change the diagnostic range to underline the `@final` on line 243 because that's where the error really is and is also the code you have to change (move the `final` to line 247)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/final.md`:305 on 2025-11-28 07:45_

Do we also need to emit diagnostic if we have something like

```py
class Chil(Bad):
    def __init__(self):
        self.f = "haha"
```



---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/final.md`:300 on 2025-11-28 07:46_

The `no diagnostic` suggests to me that we intentionally assert that we don't raise a diagnsotic here. Is that our intention? If so, why? 

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/final.md_-_Tests_for_the_`@typi…_-_Cannot_override_a_me…_(338615109711a91b).snap`:120 on 2025-11-28 07:48_

Should we omit `foo` here, given that it already appears in the primary message (and the name is also fairly obvious from what's underlined).

Overrides the definition from its superclass `Parent`

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/diagnostic.rs`:3829 on 2025-11-28 07:51_

We should assert the full concise output in at least one mdtest. I'm also not sure if it's worth here using a separate concise message. Why not always use this message and remove the primary message instead (I feel like the full output message repeats both the superclass name and member unnecessarily, we should avoid redundant information)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/function.rs`:943 on 2025-11-28 07:55_

I would a function named `first` expect to return an `Option`, which also seems more in line with what other methods iterating over overloads do. 

In general, the idea of `expect` is to have a message that says why the "expectation holds true". In this case, there isn't any. If returning an `Option` is indeed much more annoying to handle upstream, I suggest to rename the method and add a docstring saying that it's the callers responsibility to ensure this literal has at least one overload

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/final.md`:33 on 2025-11-28 07:57_

Should we add a test for a conditional member

```py
class Annoying:
    if choice():
        @final
        def test(): ...
    else:
        def test(): ...

class NowWhat(Annoying):
    def test(): ...

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/liskov.rs`:352 on 2025-11-28 07:59_

Fancy, although I think I would just have used 3 bools here. It's not like we create many instances of this and it is a bit easier to read

---

_@MichaReiser approved on 2025-11-28 07:59_

---

_@AlexWaygood reviewed on 2025-11-28 11:15_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:943 on 2025-11-28 11:15_

What would you suggest renaming this method to? Having it return an `Option` would feel very silly here. We know for a fact that there will always be at least one overload or implementation. There's no way we could possibly infer a function as being a function if it has 0 `StmtFunctionDef` nodes associated with it in the source definition.

To me `first` seems like by far the most logical name for this method, because it clearly describes what it does: it returns the first overload or implementation

---

_@AlexWaygood reviewed on 2025-11-28 11:16_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/liskov.rs`:352 on 2025-11-28 11:16_

Hmm, I find this more readable :-) and it's also more extensible: this isn't the last override-related rule we'll be adding to this module.

---

_@AlexWaygood reviewed on 2025-11-28 11:20_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/final.md`:300 on 2025-11-28 11:20_

Yes, that is our intention. I said why in the prose above this snippet: mypy and pyright do not emit a diagnostic here.

I'll flesh that description out a bit more to be more explicit that our behaviour here is mainly based on a desire to be compatible with what they do.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/final.md`:305 on 2025-11-28 11:21_

Yes, we do. We don't currently consider implicit instance attributes for any of our override-related checks, but we'll need to add that in the future. I'll add a TODO for now 👍

---

_@AlexWaygood reviewed on 2025-11-28 11:21_

---

_@AlexWaygood reviewed on 2025-11-28 11:23_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/final.md`:271 on 2025-11-28 11:23_

The diagnostic range for the `invalid-overload` diagnostic? Sure, but I'm not touching any of the `invalid-overload` logic in this PR :-) This test is just demonstrating how `override-of-final-method` diagnostics are emitted on methods that would _also_ be flagged as having invalid overloads: it's important to see how the two diagnostics interact

---

_@MichaReiser reviewed on 2025-11-28 11:24_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/function.rs`:943 on 2025-11-28 11:24_

Oh, in that case it's fine. I incorrectly assumed that we only have an `OverloadLiteral` if this is, in fact, an overload. But if this is something guaranteed by the type itself, it's fine to have the expect inside the method

---

_@AlexWaygood reviewed on 2025-11-28 11:25_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/final.md`:206 on 2025-11-28 11:25_

The overloads together all constitute a single definition of one function. I'll add some more verbiage to the diagnostic to make it clear that you'd have to remove all diagnostics for us to be satisfied 👍

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/final.md`:59 on 2025-11-28 11:25_

> Did you mean to change the decorator ordering?

Oops, yes, I did 😶

Great catch, thank you!!

---

_@AlexWaygood reviewed on 2025-11-28 11:25_

---

_@MichaReiser reviewed on 2025-11-28 11:39_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/final.md`:271 on 2025-11-28 11:39_

Yes, I'm fine leaving this to another PR then. I just got very confused by the message (which is all about `final`)

---

_@AlexWaygood reviewed on 2025-11-28 12:36_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/final.md`:271 on 2025-11-28 12:36_

I opened https://github.com/astral-sh/ty/issues/1664 to track this

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/final.md_-_Tests_for_the_`@typi…_-_A_possibly-undefined…_(fc7b496fd1986deb).snap`:249 on 2025-11-28 14:16_

@MichaReiser, I feel like I remember Ruff having this issue in the past -- can you remember what the solution was? Ideally we'd mark the autofix as display-only if we can see that it's going to introduce invalid syntax. (Or use a `range_replacement` that replaces the definition with `pass`.)

---

_@AlexWaygood reviewed on 2025-11-28 14:16_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/final.md_-_Tests_for_the_`@typi…_-_A_possibly-undefined…_(fc7b496fd1986deb).snap`:249 on 2025-11-28 14:21_

We have a `DisplayOnly` Applicability. I don't think they are shown anywhere by default (ever?). The IDE also filters out manual only edits. 


I don't think we should add fixes that introduce syntax errors. I'm also not convinced that it's the right fix. It's as likely that the proper fix is to remove the `@final`

---

_@MichaReiser reviewed on 2025-11-28 14:21_

---

_@AlexWaygood reviewed on 2025-11-28 14:22_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/final.md_-_Tests_for_the_`@typi…_-_A_possibly-undefined…_(fc7b496fd1986deb).snap`:249 on 2025-11-28 14:22_

Right, I was more wondering if Ruff had a nice little routine somewhere to easily detect that this was the only statement in the `if:` branch (and therefore that a naive deletion would introduce an unsafe fix)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:3829 on 2025-11-28 14:27_

The concise diagnostic would repeat the word "overrides" twice in one short sentence. That seems very unnecessary considering how trivial it is to set a better concise diagnostic here

---

_@AlexWaygood reviewed on 2025-11-28 14:27_

---

_@MichaReiser reviewed on 2025-11-28 14:32_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/final.md_-_Tests_for_the_`@typi…_-_A_possibly-undefined…_(fc7b496fd1986deb).snap`:249 on 2025-11-28 14:32_

https://github.com/astral-sh/ruff/blob/096e539b7249d1eada88984995233db00a2c436d/crates/ruff_linter/src/fix/edits.rs#L25-L63

---

_@AlexWaygood reviewed on 2025-11-28 14:36_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/final.md_-_Tests_for_the_`@typi…_-_A_possibly-undefined…_(fc7b496fd1986deb).snap`:249 on 2025-11-28 14:36_

ty

---

_@AlexWaygood reviewed on 2025-11-28 14:36_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/final.md_-_Tests_for_the_`@typi…_-_A_possibly-undefined…_(fc7b496fd1986deb).snap`:249 on 2025-11-28 14:36_

seems complicated, I'll leave this for a followup

---

_@MichaReiser reviewed on 2025-11-28 14:37_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/final.md_-_Tests_for_the_`@typi…_-_A_possibly-undefined…_(fc7b496fd1986deb).snap`:249 on 2025-11-28 14:37_

haha yeah

---

_@AlexWaygood reviewed on 2025-11-28 14:39_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:3829 on 2025-11-28 14:39_

I've added an explicit test for the concise message

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/final.md_-_Tests_for_the_`@typi…_-_Cannot_override_a_me…_(338615109711a91b).snap`:120 on 2025-11-28 14:39_

This actually ends up making the primary diagnostic more verbose in most cases I tested, but I agree that the repetition wasn't great. I've applied a small variant of your suggestion, thanks!

---

_@AlexWaygood reviewed on 2025-11-28 14:39_

---

_Comment by @AlexWaygood on 2025-11-28 14:40_

Thanks for the great review @MichaReiser, it flagged a bunch of edge cases I missed!

---

_Merged by @AlexWaygood on 2025-11-28 15:18_

---

_Closed by @AlexWaygood on 2025-11-28 15:18_

---

_Branch deleted on 2025-11-28 15:18_

---

_@MichaReiser reviewed on 2025-11-29 17:15_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/diagnostic.rs`:3900 on 2025-11-29 17:15_

A fix that might introduce invalid syntax should be a manual fix

---

_@AlexWaygood reviewed on 2025-12-03 11:51_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:3900 on 2025-12-03 11:51_

(fixed in https://github.com/astral-sh/ruff/pull/21699)

---
