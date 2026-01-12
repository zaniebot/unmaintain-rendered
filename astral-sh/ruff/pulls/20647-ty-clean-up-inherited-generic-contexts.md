```yaml
number: 20647
title: "[ty] Clean up inherited generic contexts"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dcreager/non-inherited
created_at: 2025-09-30T14:59:17Z
updated_at: 2025-10-03T17:55:46Z
url: https://github.com/astral-sh/ruff/pull/20647
synced_at: 2026-01-12T15:57:07Z
```

# [ty] Clean up inherited generic contexts

---

_@dcreager_

We add an `inherited_generic_context` to the constructors of a generic class. That lets us infer specializations of the class when invoking the constructor. The constructor might itself be generic, in which case we have to merge the list of typevars that we are willing to infer in the constructor call.

Before we did that by tracking the two (and their specializations) separately, with distinct `Option` fields/parameters. This PR updates our call binding logic such that any given function call has _one_ optional generic context that we're willing to infer a specialization for. If needed, we use the existing `GenericContext::merge` method to create a new combined generic context for when the class and constructor are both generic. This simplifies the call binding code considerably, and is no more complex in the constructor call logic.

We also have a heuristic that we will promote any literals in the specialized types of a generic class, but we don't promote literals in the specialized types of the function itself. To handle this, we now track this `should_promote_literals` property within `GenericContext`. And moreover, we track this separately for each typevar, instead of a single property for the generic context as a whole, so that we can correctly merge the generic context of a constructor method (where the option should be `false`) with the inherited generic context of its containing class (where the option should be `true`).

---

_Review requested from @carljm by @dcreager on 2025-09-30 14:59_

---

_Review requested from @AlexWaygood by @dcreager on 2025-09-30 14:59_

---

_Review requested from @sharkdp by @dcreager on 2025-09-30 14:59_

---

_Label `internal` added by @dcreager on 2025-09-30 14:59_

---

_Label `ty` added by @dcreager on 2025-09-30 14:59_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:144 on 2025-09-30 15:00_

This seems moot now that we have methods for walking the lexically containing contexts when needed.

---

_Comment by @github-actions[bot] on 2025-09-30 15:01_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-03 17:38:03.069240816 +0000
+++ new-output.txt	2025-10-03 17:38:06.266278635 +0000
@@ -611,7 +611,7 @@
 literals_literalstring.py:75:5: error[invalid-assignment] Object of type `Literal[b"test"]` is not assignable to `LiteralString`
 literals_literalstring.py:79:21: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `bool`
 literals_literalstring.py:120:22: error[invalid-argument-type] Argument to function `literal_identity` is incorrect: Argument type `str` does not satisfy upper bound `LiteralString` of type variable `TLiteral`
-literals_literalstring.py:130:5: error[invalid-assignment] Object of type `Container[str]` is not assignable to `Container[LiteralString]`
+literals_literalstring.py:130:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Container[T@Container]`, found `Container[str]`
 literals_literalstring.py:134:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `str` does not satisfy upper bound `LiteralString` of type variable `T`
 literals_literalstring.py:138:1: error[invalid-assignment] Object of type `list[Unknown | str]` is not assignable to `list[LiteralString]`
 literals_literalstring.py:167:1: error[type-assertion-failure] Argument does not have asserted type `A`
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-30 15:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas (https://github.com/pandas-dev/pandas)
- pandas/io/json/_json.py:816:16: error[no-matching-overload] No overload of bound method `read` matches arguments
+ pandas/io/json/_json.py:792:19: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `JsonReader[FrameSeriesStrT@JsonReader]`, found `JsonReader[str]`
+ pandas/tests/io/json/test_readlines.py:221:18: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `JsonReader[FrameSeriesStrT@JsonReader]`, found `JsonReader[str]`
- pandas/tests/io/json/test_readlines.py:237:14: error[invalid-context-manager] Object of type `JsonReader[str]` cannot be used with `with` because it does not correctly implement `__enter__` or `__exit__`
- pandas/tests/io/json/test_readlines.py:238:13: error[no-matching-overload] No overload of bound method `read` matches arguments
- pandas/tests/scalar/timedelta/test_arithmetic.py:183:18: error[unsupported-operator] Operator `-` is unsupported between objects of type `Timedelta` and `timedelta64[str]`
- pandas/tests/scalar/timedelta/test_arithmetic.py:186:18: error[unsupported-operator] Operator `-` is unsupported between objects of type `timedelta64[str]` and `Timedelta`
- pandas/tests/scalar/timedelta/test_arithmetic.py:551:18: error[unsupported-operator] Operator `/` is unsupported between objects of type `timedelta64[str]` and `Timedelta`
+ pandas/tests/scalar/timedelta/test_arithmetic.py:551:18: error[unsupported-operator] Operator `/` is unsupported between objects of type `timedelta64[int]` and `Timedelta`
- Found 3390 diagnostics
+ Found 3387 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/core.py:1566:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Event[_DataT@Event]`, found `Event[_DataT@async_fire_internal | None]`
- Found 13715 diagnostics
+ Found 13716 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     struct fields = ~35MB
+     struct fields = ~33MB

```
</details>


---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/function.rs`:480 on 2025-09-30 15:10_

`FunctionLiteral` now only contains one field, so there's an even better argument to consolidate some of the types in this file.

---

_@dcreager reviewed on 2025-09-30 15:10_

---

_Comment by @dcreager on 2025-09-30 15:10_

Moving this to draft while I look at the ecosystem results

---

_Converted to draft by @dcreager on 2025-09-30 15:10_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-09-30 18:05_

---

_Comment by @github-actions[bot] on 2025-09-30 18:11_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 3 | 0 | 0 |
| `unsupported-operator` | 0 | 2 | 1 |
| `no-matching-overload` | 0 | 2 | 0 |
| `invalid-context-manager` | 0 | 1 | 0 |
| **Total** | **3** | **5** | **1** |

**[Full report with detailed diff](https://dcreager-non-inherited.ecosystem-663.pages.dev/diff)** ([timing results](https://dcreager-non-inherited.ecosystem-663.pages.dev/timing))


---

_Comment by @dcreager on 2025-10-01 15:00_

> Moving this to draft while I look at the ecosystem results

The issue was with this part of my original PR description:

> We also have a heuristic that we will promote any literals in the specialized types of a generic class, but we don't promote literals in the specialized types of the function itself. This PR also has the nice side benefit that that heuristic is now implemented in the constructor call machinery (where is arguably a better place for it) rather than in the general call binding machinery.

It turns out that we _need_ to perform literal promotion as part of the call binding machinery; otherwise we are using the literal types (not the promoted types) when doing type checking of arguments against parameter annotations.

That complicates this PR slightly, since we now need some other mechanism to track whether we want to perform literal promotion on each typevar. The premise of this PR is that we want a single `GenericContext` to hold the entire "inference context" of a particular call site, merging together the generic context of a constructor and its containing class if needed. That means that we need to track this option on a per-typevar basis, not a per-generic-context basis.

I had considered doing that by adding a `should_promote_literals` field to `BoundTypeVarInstance`, but that makes typevar equality checks (and by extension, looking up the specialization of a particular typevar in a `Specialization`) more brittle. So instead, I've updated `GenericContext` to hold a small options struct for each typevar, and put the `should_promote_literals` field there. When accessing a constructor member, we create a copy of the class's generic context where we update that option to `true` for all of the typevars in the generic context. When that is merged with the generic context of the constructor itself (if any), the inherited typevars from the class have the option set, and the typevars from the method do not.

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-10-01 15:00_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-10-01 15:00_

---

_Comment by @github-actions[bot] on 2025-10-01 15:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dcreager on 2025-10-01 18:36_

The remaining ecosystem results look correct. Or at least, they're known false positives that we're now picking up because of inferring generic class typevars more correctly.

---

`home-assistant`

[Here](https://github.com/home-assistant/core/blob/fd8ccb8d8f6749c3672168fc8bf2bfe28481e16b/homeassistant/core.py#L1566) we're calling the `Event[_DataT]` constructor, but our narrowing constraints aren't noticing that `event_data` cannot be `None`. We need to either fix the narrowing logic to eliminate `None` from the union, or rely on the better constraint solver to infer a more precise type for `_DataT`.

---

`pandas`

The `json`-related diagnostics are an interesting case where our literal promotion logic is not what the user would want. They are using literal strings as type tags in the `JSONReader` class, and when they instantiate a `JSONReader` with `typ="frame"`, they _want_ the inferred type to be `JSONReader[Literal["frame"]]`, not `JSONReader[str]`. But our literal promotion logic engages (correctly, where it should have been already doing so).

For the `timedelta` diagnostics, they are [instantiating](https://github.com/pandas-dev/pandas/blob/5cc32409652002b4de442cfa6a065553aa17c3c9/pandas/tests/scalar/timedelta/test_arithmetic.py#L181) a `numpy.timedelta64` using a [dedicated constructor overload](https://github.com/numpy/numpy/blob/e7276da07f6fc69cc2947f4dc67a87472e4b3a8b/numpy/__init__.pyi#L5208) that matches when the constructor parameter is a particular literal string. Our literal promotion is engaging before we check overloads, so we don't match that overload.

---

_Marked ready for review by @dcreager on 2025-10-02 01:40_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5479 on 2025-10-03 12:32_

I think you could improve readability here by factoring out a closure:


```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 1ab2447b5e..05dc47c3ff 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -5459,24 +5459,22 @@ impl<'db> Type<'db> {
                     }
                 }
 
-                let new_specialization = new_call_outcome
-                    .and_then(Result::ok)
-                    .as_ref()
-                    .and_then(Bindings::single_element)
-                    .into_iter()
-                    .flat_map(CallableBinding::matching_overloads)
-                    .next()
-                    .and_then(|(_, binding)| binding.specialization())
-                    .and_then(|specialization| specialization.restrict(db, generic_context?));
-                let init_specialization = init_call_outcome
-                    .and_then(Result::ok)
-                    .as_ref()
-                    .and_then(Bindings::single_element)
-                    .into_iter()
-                    .flat_map(CallableBinding::matching_overloads)
-                    .next()
-                    .and_then(|(_, binding)| binding.specialization())
-                    .and_then(|specialization| specialization.restrict(db, generic_context?));
+                let specialize_constructor = |outcome: Option<Bindings<'db>>| {
+                    let generic_context = generic_context?;
+
+                    let (_, binding) = outcome
+                        .as_ref()?
+                        .single_element()?
+                        .matching_overloads()
+                        .next()?;
+
+                    binding.specialization()?.restrict(db, generic_context)
+                };
+
+                let new_specialization =
+                    specialize_constructor(new_call_outcome.and_then(Result::ok));
+                let init_specialization =
+                    specialize_constructor(init_call_outcome.and_then(Result::ok));
                 let specialization =
                     combine_specializations(db, new_specialization, init_specialization);
                 let specialized = specialization
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:241 on 2025-10-03 12:38_

```suggestion
        self.variables_inner(db).keys().copied()
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:5369 on 2025-10-03 12:45_

maybe rewrite this as a closure?


```diff
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index 77bc4578b1..0462d31c70 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -5351,19 +5351,13 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
         collection_class: KnownClass,
     ) -> Option<Type<'db>> {
         // Extract the type variable `T` from `list[T]` in typeshed.
-        fn elt_tys(
-            collection_class: KnownClass,
-            db: &dyn Db,
-        ) -> Option<(
-            ClassLiteral<'_>,
-            impl Iterator<Item = BoundTypeVarInstance<'_>> + Clone,
-        )> {
-            let class_literal = collection_class.try_to_class_literal(db)?;
-            let generic_context = class_literal.generic_context(db)?;
-            Some((class_literal, generic_context.variables(db)))
-        }
-
-        let (class_literal, elt_tys) = elt_tys(collection_class, self.db()).unwrap_or_else(|| {
+        let elt_tys = |collection_class: KnownClass| {
+            let class_literal = collection_class.try_to_class_literal(self.db())?;
+            let generic_context = class_literal.generic_context(self.db())?;
+            Some((class_literal, generic_context.variables(self.db())))
+        };
+
+        let (class_literal, elt_tys) = elt_tys(collection_class).unwrap_or_else(|| {
             let name = collection_class.name(self.db());
             panic!("Typeshed should always have a `{name}` class in `builtins.pyi`")
         });
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:698 on 2025-10-03 12:48_

we no longer need this call because `FunctionLiteral` no longer contains any non-atomic types within it that would need to be walked?

---

_@AlexWaygood approved on 2025-10-03 12:50_

I think I mostly understand this 😅

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/function.rs`:698 on 2025-10-03 17:26_

Yep, that's right

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:5479 on 2025-10-03 17:31_

I like

---

_@dcreager reviewed on 2025-10-03 17:34_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer/builder.rs`:5369 on 2025-10-03 17:35_

Done

---

_@dcreager reviewed on 2025-10-03 17:35_

---

_Comment by @codspeed-hq[bot] on 2025-10-03 17:44_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fnon-inherited?runnerMode=Instrumentation)

### Merging #20647 will **improve performances by 26.18%**

<sub>Comparing <code>dcreager/non-inherited</code> (407f7bb) with <code>main</code> (673167a)[^unexpected-base]</sub>
[^unexpected-base]: No successful run was found on <code>main</code> (c91b457) during the generation of this report, so 673167a was used instead as the comparison base. There might be some changes unrelated to this pull request in this report.



### Summary

`⚡ 3` improvements  
`✅ 40` untouched  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` attrs `` | 431.9 ms | 388.2 ms | +11.25% |
| ⚡ | `` DateType `` | 235.5 ms | 186.7 ms | +26.18% |
| ⚡ | `` hydra-zen `` | 851.9 ms | 763.9 ms | +11.53% |


---

_Comment by @AlexWaygood on 2025-10-03 17:49_

> 1. No successful run was found on `main` ([c91b457](https://github.com/astral-sh/ruff/commit/c91b4570448abf9871f29476fbcf822abd1afd92)) during the generation of this report, so 673167a was used instead as the comparison base. There might be some changes unrelated to this pull request in this report. [↩](#user-content-fnref-unexpected-base-5abce3b6bd8bc785d5787d8c21019003)

I suspect Codspeed _might_ have picked up some of the perf improvements from https://github.com/astral-sh/ruff/commit/c91b4570448abf9871f29476fbcf822abd1afd92 accidentally there haha

---

_Merged by @dcreager on 2025-10-03 17:55_

---

_Closed by @dcreager on 2025-10-03 17:55_

---

_Branch deleted on 2025-10-03 17:55_

---
