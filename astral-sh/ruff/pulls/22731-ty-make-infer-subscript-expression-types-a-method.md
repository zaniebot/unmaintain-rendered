```yaml
number: 22731
title: "[ty] Make `infer_subscript_expression_types` a method on `Type`"
type: pull_request
state: open
author: charliermarsh
labels:
  - internal
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: charlie/int-method
created_at: 2026-01-19T18:58:41Z
updated_at: 2026-01-20T12:07:22Z
url: https://github.com/astral-sh/ruff/pull/22731
synced_at: 2026-01-20T12:35:50Z
```

# [ty] Make `infer_subscript_expression_types` a method on `Type`

---

_@charliermarsh_

## Summary

A refactor in anticipation of https://github.com/astral-sh/ruff/pull/22654.

---

_Label `ty` added by @charliermarsh on 2026-01-19 18:59_

---

_Label `internal` added by @charliermarsh on 2026-01-19 18:59_

---

_Label `ecosystem-analyzer` added by @charliermarsh on 2026-01-19 18:59_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 19:02_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
- Found 5406 diagnostics
+ Found 5411 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 46 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1825 diagnostics
+ Found 1821 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 4449 diagnostics
+ Found 4451 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14491 diagnostics
+ Found 14492 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-19 19:02_

---

_Review requested from @carljm by @charliermarsh on 2026-01-19 19:02_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-19 19:02_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-19 19:02_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-19 19:02_

---

_Comment by @charliermarsh on 2026-01-19 19:03_

Okay, looks like a no-op as expected (those look like the usual suspects in mypy-primer).

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 19:04_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 0 | 4 | 6 |
| `invalid-argument-type` | 2 | 1 | 5 |
| `invalid-parameter-default` | 0 | 0 | 7 |
| `invalid-assignment` | 0 | 0 | 5 |
| `unused-ignore-comment` | 0 | 2 | 0 |
| **Total** | **2** | **7** | **23** |


**[Full report with detailed diff](https://36f9a527.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://36f9a527.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @astral-sh-bot[bot] on 2026-01-19 19:05_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_@MichaReiser reviewed on 2026-01-19 19:11_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:3789 on 2026-01-19 19:11_

We don't really use the `infer` terminology on `Type`. In general, the methods on `Type` are expressed in type operations. E.g `Type::subscript` would be a good fit, and it would be a salsa query. 



---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:9721 on 2026-01-19 19:11_

What do you think of moving all this plentyfull new code to `subscript.rs` to not make `types.rs` even bigg

---

_@MichaReiser reviewed on 2026-01-19 19:12_

---

_@AlexWaygood reviewed on 2026-01-19 19:15_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3789 on 2026-01-19 19:15_

> it would be a salsa query

Would it? `Type::try_iterate()` and `Type::try_call()` are not Salsa queries. I think most methods like this are _not_ Salsa queries?

---

_@AlexWaygood reviewed on 2026-01-19 19:16_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9721 on 2026-01-19 19:16_

yes, I agree -- or even an entirely new module :-) though `subscript.rs` _is_ very well named for what this new code does...

---

_@AlexWaygood reviewed on 2026-01-19 19:21_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3789 on 2026-01-19 19:21_

I agree that `Type::subscript()` is what we'd usually call this, though

---

_@charliermarsh reviewed on 2026-01-19 19:26_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types.rs`:9721 on 2026-01-19 19:26_

Aye aye!

---

_@charliermarsh reviewed on 2026-01-19 19:28_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types.rs`:3789 on 2026-01-19 19:28_

I renamed it.

---

_@charliermarsh reviewed on 2026-01-19 19:28_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types.rs`:9721 on 2026-01-19 19:28_

I moved it into `types/subscript.rs` and left the top-level `subscript.rs` as-is (I suppose that could be renamed to `index` or `pyindex` though).

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/subscript.rs`:1 on 2026-01-19 19:28_

Oh, I was thinking we could put the new `Type` method in this module too? We have to find some way to split up `types.rs` 😆 and there's precedent now with all the type-relation methods being in `types/relation.rs`

---

_@AlexWaygood reviewed on 2026-01-19 19:28_

---

_@charliermarsh reviewed on 2026-01-19 19:29_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/subscript.rs`:1 on 2026-01-19 19:29_

I totally defer to you, I didn't realize we had `impl` blocks outside of `types.rs`.

---

_@AlexWaygood reviewed on 2026-01-19 19:35_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/subscript.rs`:1 on 2026-01-19 19:35_

I say let's pull it all out of `types.rs`! Both `types.rs` and `infer/builder.rs` are monster files where GitHub now struggles with syntax highlighting; it's a medium-term goal to reduce them both in size

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-19 20:26_

---

_Comment by @AlexWaygood on 2026-01-19 20:31_

(will review tomorrow morning -- thank you!)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:3789 on 2026-01-20 06:55_

`try_call` is different. It doesn't accept any AST-dependent arguments, nor does it access the `semantic_index`. It entirely operates on the Type-IR. 

Which is very different to what we have here with

```
		expr_context: ast::ExprContext,
        scope_id: ScopeId<'db>,
        index: &'db SemanticIndex<'db>,
        typevar_binding_context: Option<Definition<'db>>,
```

But I also find it difficult to give any advice here with this little information in the summary. Where and how do we plan on using this? 

---

_@MichaReiser reviewed on 2026-01-20 06:55_

---

_@AlexWaygood reviewed on 2026-01-20 08:39_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3789 on 2026-01-20 08:39_

Ah I think this new method should probably operate on the Type IR too (good catch, I haven't really looked at the code yet).



---

_@AlexWaygood reviewed on 2026-01-20 11:30_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3789 on 2026-01-20 11:30_

So the only AST node passed in here is `ast::ExprContext`, which doesn't hold a `TextRange` or node index in it. So I don't _think_ that this should cause any issues with overly eager Salsa invalidation?

https://github.com/astral-sh/ruff/blob/0a1dddbf73e368a2019617ee907b0eb271eca979/crates/ruff_python_ast/src/nodes.rs#L2478-L2486

If even passing this in as an argument could cause issues, though, then it could be easily replaced with a boolean `in_store_context` parameter. All we do with it is check that `expr_context != ExprContext::Store` towards the end of the method.

Passing in the `scope_id`, `index` and `typevar_binding_context` are different, though... and I don't see a way to avoid that, given how special-cased several subscript operations are in the Python type system. So it may well be best to make this a Salsa-tracked query, like @MichaReiser suggests.

---

_@AlexWaygood reviewed on 2026-01-20 11:35_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3789 on 2026-01-20 11:35_

> But I also find it difficult to give any advice here with this little information in the summary. Where and how do we plan on using this?

The main motivation is to make this a "pure" method that does not have side effects (does not eagerly emit diagnostics), and instead returns a `Result` that allows the caller to either emit a diagnostic or discard the error, as it sees fit.

The new design will make it much easier to implement subscript support for intersections (https://github.com/astral-sh/ruff/pull/22654), because for subscript support on intersections we need to speculatively call `Type::subscript()` on each intersection element, and only report a diagnostic if the call would fail on _each_ intersection element.

This new design will also make it easier to improve our subscript diagnostics on union types in the future. Right now if you have an object of type `int | range` and you try to subscript it, ty will report two diagnostics -- one for subscripting an object of type `int` and one for subscripting an object of type `range`. Ideally we would combine these two into a single diagnostic, but that's hard with the current `TypeInferencBuilder::infer_subscript_expression_types()` method that eagerly emits diagnostics as soon as it encounters them.

---

_@MichaReiser reviewed on 2026-01-20 11:59_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:3789 on 2026-01-20 11:59_

Always making this a salsa query might negatively impact memory usage and isn't really necessary when called in the type inference builder. 

I wonder if this should just be a standalone helper function instead that we call from the type inference builder (uncached), and we can later add a `try_subscript` with whatever we exactly need in `Type::subscript.` 

---

_@AlexWaygood reviewed on 2026-01-20 12:07_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3789 on 2026-01-20 12:07_

Okay, so I think we can make this a pure method that doesn't depend on state (like `Type::try_call`, `Type::try_iterate`, etc.) by moving the special cases for `Generic[]` subscripts, `Protocol` subscripts and `Concatenate` subscripts out of the method and back into the `TypeInferenceBuilder`. This patch passes all tests, and means that the `Type` method no longer needs to have `index`, `typevar_binding_context` or `scope` passed in. Unlike the special cases we apply for tuple subscripts, string-literal subscripts, etc., it isn't important for us to apply these special cases recursively: it's not necessary for us to recognise the very special nature of these special forms in the very unlikely event that they appear in a union or intersection:

<details>

```diff
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index ce93043720..71f703ef62 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -109,6 +109,7 @@ use crate::types::infer::nearest_enclosing_function;
 use crate::types::mro::{DynamicMroErrorKind, StaticMroErrorKind};
 use crate::types::newtype::NewType;
 use crate::types::subclass_of::SubclassOfInner;
+use crate::types::subscript::{SubscriptError, SubscriptErrorKind};
 use crate::types::tuple::{Tuple, TupleLength, TupleSpec, TupleSpecBuilder, TupleType};
 use crate::types::typed_dict::{
     TypedDictAssignmentKind, validate_typed_dict_constructor, validate_typed_dict_dict_literal,
@@ -123,7 +124,7 @@ use crate::types::{
     TrackedConstraintSet, Truthiness, Type, TypeAliasType, TypeAndQualifiers, TypeContext,
     TypeQualifiers, TypeVarBoundOrConstraints, TypeVarBoundOrConstraintsEvaluation,
     TypeVarDefaultEvaluation, TypeVarIdentity, TypeVarInstance, TypeVarKind, TypeVarVariance,
-    TypedDictType, UnionBuilder, UnionType, UnionTypeInstance, binding_type,
+    TypedDictType, UnionBuilder, UnionType, UnionTypeInstance, any_over_type, binding_type,
     definition_expression_type, infer_scope_types, todo_type,
 };
 use crate::types::{CallableTypes, overrides};
@@ -14453,20 +14454,141 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
         slice_ty: Type<'db>,
         expr_context: ExprContext,
     ) -> Type<'db> {
-        match value_ty.subscript(
-            self.db(),
-            slice_ty,
-            expr_context,
-            self.scope(),
-            self.index,
-            self.typevar_binding_context,
-        ) {
-            Ok(result) => result,
-            Err(error) => {
-                error.report_diagnostics(&self.context, subscript);
-                error.result_type()
+        #[derive(Debug, Clone, Copy, PartialEq, Eq)]
+        enum LegacyGenericContextError<'db> {
+            /// It's invalid to subscript `Generic` or `Protocol` with this type.
+            InvalidArgument(Type<'db>),
+            /// It's invalid to subscript `Generic` or `Protocol` with a variadic tuple type.
+            /// We should emit a diagnostic for this, but we don't yet.
+            VariadicTupleArguments,
+            /// It's valid to subscribe `Generic` or `Protocol` with this type,
+            /// but the type is not yet supported.
+            NotYetSupported,
+        }
+
+        impl<'db> LegacyGenericContextError<'db> {
+            const fn into_type(self) -> Type<'db> {
+                match self {
+                    LegacyGenericContextError::InvalidArgument(_)
+                    | LegacyGenericContextError::VariadicTupleArguments => Type::unknown(),
+                    LegacyGenericContextError::NotYetSupported => {
+                        todo_type!("ParamSpecs and TypeVarTuples")
+                    }
+                }
             }
         }
+
+        let db = self.db();
+
+        let legacy_generic_class_context = |typevars: Type<'db>| -> Result<
+            GenericContext<'db>,
+            LegacyGenericContextError<'db>,
+        > {
+            let typevars_class_tuple_spec = typevars.exact_tuple_instance_spec(db);
+
+            let typevars = if let Some(tuple_spec) = typevars_class_tuple_spec.as_deref() {
+                match tuple_spec {
+                    Tuple::Fixed(typevars) => typevars.elements_slice(),
+                    Tuple::Variable(_) => {
+                        return Err(LegacyGenericContextError::VariadicTupleArguments);
+                    }
+                }
+            } else {
+                std::slice::from_ref(&typevars)
+            };
+
+            let typevars: Result<FxOrderSet<_>, LegacyGenericContextError<'db>> = typevars
+                .iter()
+                .map(|typevar| {
+                    let argument_ty = *typevar;
+                    if let Type::KnownInstance(KnownInstanceType::TypeVar(typevar)) = argument_ty {
+                        bind_typevar(
+                            db,
+                            self.index,
+                            self.scope().file_scope_id(db),
+                            self.typevar_binding_context,
+                            typevar,
+                        )
+                        .ok_or(LegacyGenericContextError::InvalidArgument(argument_ty))
+                    } else if any_over_type(
+                        db,
+                        argument_ty,
+                        &|ty| match ty {
+                            Type::Dynamic(
+                                DynamicType::TodoUnpack | DynamicType::TodoStarredExpression,
+                            ) => true,
+                            Type::NominalInstance(nominal) => {
+                                nominal.has_known_class(db, KnownClass::TypeVarTuple)
+                            }
+                            _ => false,
+                        },
+                        true,
+                    ) {
+                        Err(LegacyGenericContextError::NotYetSupported)
+                    } else {
+                        Err(LegacyGenericContextError::InvalidArgument(argument_ty))
+                    }
+                })
+                .collect();
+            typevars.map(|typevars| GenericContext::from_typevar_instances(db, typevars))
+        };
+
+        // Special typing forms for which subscriptions are context-dependent are parsed here,
+        // outside of `Type::subscript`, which is a pure function that doesn't depend on the
+        // semantic index or any context-dependent state.
+        let subscript_result = match value_ty {
+            Type::SpecialForm(SpecialFormType::Generic) => {
+                match legacy_generic_class_context(slice_ty) {
+                    Ok(context) => Ok(Type::KnownInstance(KnownInstanceType::SubscriptedGeneric(
+                        context,
+                    ))),
+                    Err(LegacyGenericContextError::InvalidArgument(argument_ty)) => {
+                        Err(SubscriptError::new(
+                            Type::unknown(),
+                            SubscriptErrorKind::InvalidLegacyGenericArgument {
+                                origin: "Generic",
+                                argument_ty,
+                            },
+                        ))
+                    }
+                    Err(error) => Ok(error.into_type()),
+                }
+            }
+            Type::SpecialForm(SpecialFormType::Protocol) => {
+                match legacy_generic_class_context(slice_ty) {
+                    Ok(context) => Ok(Type::KnownInstance(KnownInstanceType::SubscriptedProtocol(
+                        context,
+                    ))),
+                    Err(LegacyGenericContextError::InvalidArgument(argument_ty)) => {
+                        Err(SubscriptError::new(
+                            Type::unknown(),
+                            SubscriptErrorKind::InvalidLegacyGenericArgument {
+                                origin: "Protocol",
+                                argument_ty,
+                            },
+                        ))
+                    }
+                    Err(error) => Ok(error.into_type()),
+                }
+            }
+            Type::SpecialForm(SpecialFormType::Concatenate) => {
+                // TODO: Add proper support for `Concatenate`
+                let mut variables = FxOrderSet::default();
+                slice_ty.bind_and_find_all_legacy_typevars(
+                    db,
+                    self.typevar_binding_context,
+                    &mut variables,
+                );
+                let generic_context = GenericContext::from_typevar_instances(db, variables);
+                Ok(Type::Dynamic(DynamicType::UnknownGeneric(generic_context)))
+            }
+            _ => value_ty.subscript(self.db(), slice_ty, expr_context),
+        };
+
+        subscript_result.unwrap_or_else(|e| {
+            e.report_diagnostics(&self.context, subscript);
+            e.result_type()
+        })
     }
 
     fn infer_slice_expression(&mut self, slice: &ast::ExprSlice) -> Type<'db> {
diff --git a/crates/ty_python_semantic/src/types/subscript.rs b/crates/ty_python_semantic/src/types/subscript.rs
index 9397e7e3e3..e78465d401 100644
--- a/crates/ty_python_semantic/src/types/subscript.rs
+++ b/crates/ty_python_semantic/src/types/subscript.rs
@@ -1,12 +1,9 @@
 use itertools::Itertools;
 use ruff_python_ast as ast;
 
+use crate::Db;
 use crate::place::{DefinedPlace, Definedness, Place};
-use crate::semantic_index::SemanticIndex;
-use crate::semantic_index::definition::Definition;
-use crate::semantic_index::scope::ScopeId;
 use crate::subscript::{PyIndex, PySlice};
-use crate::{Db, FxOrderSet};
 
 use super::call::{Bindings, CallArguments, CallDunderError, CallError, CallErrorKind};
 use super::class::KnownClass;
@@ -17,12 +14,10 @@ use super::diagnostic::{
     report_index_out_of_bounds, report_invalid_key_on_typed_dict, report_not_subscriptable,
     report_slice_step_size_zero,
 };
-use super::generics::{GenericContext, bind_typevar};
 use super::infer::TypeContext;
 use super::instance::SliceLiteral;
 use super::special_form::SpecialFormType;
-use super::tuple::{Tuple, TupleSpec};
-use super::visitor::any_over_type;
+use super::tuple::TupleSpec;
 use super::{
     DynamicType, KnownInstanceType, Type, TypeAliasType, UnionBuilder, UnionType, todo_type,
 };
@@ -34,7 +29,7 @@ pub(crate) struct SubscriptError<'db> {
 }
 
 #[derive(Debug)]
-enum SubscriptErrorKind<'db> {
+pub(crate) enum SubscriptErrorKind<'db> {
     /// An index is out of bounds for a literal tuple/string/bytes subscript.
     IndexOutOfBounds {
         kind: &'static str,
@@ -83,7 +78,7 @@ enum SubscriptErrorKind<'db> {
 }
 
 impl<'db> SubscriptError<'db> {
-    fn new(result_ty: Type<'db>, error: SubscriptErrorKind<'db>) -> Self {
+    pub(crate) fn new(result_ty: Type<'db>, error: SubscriptErrorKind<'db>) -> Self {
         Self {
             result_ty,
             errors: vec![error],
@@ -293,87 +288,7 @@ impl<'db> Type<'db> {
         db: &'db dyn Db,
         slice_ty: Type<'db>,
         expr_context: ast::ExprContext,
-        scope_id: ScopeId<'db>,
-        index: &'db SemanticIndex<'db>,
-        typevar_binding_context: Option<Definition<'db>>,
     ) -> Result<Type<'db>, SubscriptError<'db>> {
-        #[derive(Debug, Clone, Copy, PartialEq, Eq)]
-        enum LegacyGenericContextError<'db> {
-            /// It's invalid to subscript `Generic` or `Protocol` with this type.
-            InvalidArgument(Type<'db>),
-            /// It's invalid to subscript `Generic` or `Protocol` with a variadic tuple type.
-            /// We should emit a diagnostic for this, but we don't yet.
-            VariadicTupleArguments,
-            /// It's valid to subscribe `Generic` or `Protocol` with this type,
-            /// but the type is not yet supported.
-            NotYetSupported,
-        }
-
-        impl<'db> LegacyGenericContextError<'db> {
-            const fn into_type(self) -> Type<'db> {
-                match self {
-                    LegacyGenericContextError::InvalidArgument(_)
-                    | LegacyGenericContextError::VariadicTupleArguments => Type::unknown(),
-                    LegacyGenericContextError::NotYetSupported => {
-                        todo_type!("ParamSpecs and TypeVarTuples")
-                    }
-                }
-            }
-        }
-
-        let legacy_generic_class_context = |typevars: Type<'db>| -> Result<
-            GenericContext<'db>,
-            LegacyGenericContextError<'db>,
-        > {
-            let typevars_class_tuple_spec = typevars.exact_tuple_instance_spec(db);
-
-            let typevars = if let Some(tuple_spec) = typevars_class_tuple_spec.as_deref() {
-                match tuple_spec {
-                    Tuple::Fixed(typevars) => typevars.elements_slice(),
-                    Tuple::Variable(_) => {
-                        return Err(LegacyGenericContextError::VariadicTupleArguments);
-                    }
-                }
-            } else {
-                std::slice::from_ref(&typevars)
-            };
-
-            let typevars: Result<FxOrderSet<_>, LegacyGenericContextError<'db>> = typevars
-                .iter()
-                .map(|typevar| {
-                    let argument_ty = *typevar;
-                    if let Type::KnownInstance(KnownInstanceType::TypeVar(typevar)) = argument_ty {
-                        bind_typevar(
-                            db,
-                            index,
-                            scope_id.file_scope_id(db),
-                            typevar_binding_context,
-                            typevar,
-                        )
-                        .ok_or(LegacyGenericContextError::InvalidArgument(argument_ty))
-                    } else if any_over_type(
-                        db,
-                        argument_ty,
-                        &|ty| match ty {
-                            Type::Dynamic(
-                                DynamicType::TodoUnpack | DynamicType::TodoStarredExpression,
-                            ) => true,
-                            Type::NominalInstance(nominal) => {
-                                nominal.has_known_class(db, KnownClass::TypeVarTuple)
-                            }
-                            _ => false,
-                        },
-                        true,
-                    ) {
-                        Err(LegacyGenericContextError::NotYetSupported)
-                    } else {
-                        Err(LegacyGenericContextError::InvalidArgument(argument_ty))
-                    }
-                })
-                .collect();
-            typevars.map(|typevars| GenericContext::from_typevar_instances(db, typevars))
-        };
-
         let value_ty = self;
 
         let inferred = match (value_ty, slice_ty) {
@@ -383,18 +298,12 @@ impl<'db> Type<'db> {
                 db,
                 slice_ty,
                 expr_context,
-                scope_id,
-                index,
-                typevar_binding_context,
             )),
 
             (_, Type::TypeAlias(alias)) => Some(value_ty.subscript(
                 db,
                 alias.value_type(db),
                 expr_context,
-                scope_id,
-                index,
-                typevar_binding_context,
             )),
 
             (Type::Union(union), _) => Some(map_union_subscript(db, union, |element| {
@@ -402,9 +311,6 @@ impl<'db> Type<'db> {
                     db,
                     slice_ty,
                     expr_context,
-                    scope_id,
-                    index,
-                    typevar_binding_context,
                 )
             })),
 
@@ -413,9 +319,6 @@ impl<'db> Type<'db> {
                     db,
                     element,
                     expr_context,
-                    scope_id,
-                    index,
-                    typevar_binding_context,
                 )
             })),
 
@@ -556,9 +459,6 @@ impl<'db> Type<'db> {
                     db,
                     Type::IntLiteral(i64::from(bool)),
                     expr_context,
-                    scope_id,
-                    index,
-                    typevar_binding_context,
                 ))
             }
 
@@ -569,30 +469,9 @@ impl<'db> Type<'db> {
                     db,
                     Type::IntLiteral(i64::from(bool)),
                     expr_context,
-                    scope_id,
-                    index,
-                    typevar_binding_context,
                 ))
             }
 
-            (Type::SpecialForm(SpecialFormType::Protocol), typevars) => Some(
-                match legacy_generic_class_context(typevars) {
-                    Ok(context) => Ok(Type::KnownInstance(
-                        KnownInstanceType::SubscriptedProtocol(context),
-                    )),
-                    Err(LegacyGenericContextError::InvalidArgument(argument_ty)) => Err(
-                        SubscriptError::new(
-                            Type::unknown(),
-                            SubscriptErrorKind::InvalidLegacyGenericArgument {
-                                origin: "Protocol",
-                                argument_ty,
-                            },
-                        ),
-                    ),
-                    Err(error) => Ok(error.into_type()),
-                },
-            ),
-
             (Type::KnownInstance(KnownInstanceType::SubscriptedProtocol(_)), _) => {
                 // TODO: emit a diagnostic
                 Some(Ok(todo_type!("doubly-specialized typing.Protocol")))
@@ -611,24 +490,6 @@ impl<'db> Type<'db> {
                 )))
             }
 
-            (Type::SpecialForm(SpecialFormType::Generic), typevars) => Some(
-                match legacy_generic_class_context(typevars) {
-                    Ok(context) => Ok(Type::KnownInstance(
-                        KnownInstanceType::SubscriptedGeneric(context),
-                    )),
-                    Err(LegacyGenericContextError::InvalidArgument(argument_ty)) => Err(
-                        SubscriptError::new(
-                            Type::unknown(),
-                            SubscriptErrorKind::InvalidLegacyGenericArgument {
-                                origin: "Generic",
-                                argument_ty,
-                            },
-                        ),
-                    ),
-                    Err(error) => Ok(error.into_type()),
-                },
-            ),
-
             (Type::KnownInstance(KnownInstanceType::SubscriptedGeneric(_)), _) => {
                 // TODO: emit a diagnostic
                 Some(Ok(todo_type!("doubly-specialized typing.Generic")))
@@ -638,18 +499,6 @@ impl<'db> Type<'db> {
                 Some(Ok(Type::Dynamic(DynamicType::TodoUnpack)))
             }
 
-            (Type::SpecialForm(SpecialFormType::Concatenate), _) => {
-                // TODO: Add proper support for `Concatenate`
-                let mut variables = FxOrderSet::default();
-                slice_ty.bind_and_find_all_legacy_typevars(
-                    db,
-                    typevar_binding_context,
-                    &mut variables,
-                );
-                let generic_context = GenericContext::from_typevar_instances(db, variables);
-                Some(Ok(Type::Dynamic(DynamicType::UnknownGeneric(generic_context))))
-            }
-
             (Type::SpecialForm(special_form), _) if special_form.class().is_special_form() => {
                 Some(Ok(todo_type!("Inference of subscript on special form")))
             }
```

</details>

Then we don't need to add any Salsa caching, and we end up with a very similar design to what we use elsewhere for recursive operations on `Type`s.

---
