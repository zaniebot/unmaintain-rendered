```yaml
number: 13764
title: "Add scope assertion to `TypeInference.extend`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/assert-type-inference-scopes-when-extending
created_at: 2024-10-15T15:22:10Z
updated_at: 2024-10-16T15:44:27Z
url: https://github.com/astral-sh/ruff/pull/13764
synced_at: 2026-01-12T15:55:45Z
```

# Add scope assertion to `TypeInference.extend`

---

_@MichaReiser_

## Summary

I thought this is an easy change, but many tests are now blowing up. I or someone else has to tacke a second look why and where
we're merging `TypeInference` results from different scopes. 

This PR adds a debug assertion that asserts that `TypeInference::extend` is only called on results that have the same scope. 
This is critical because `expressions` uses `ScopedExpressionId` that are local and merging expressions from different
scopes would lead to incorrect expression types.

We could consider storing `scope` only on `TypeInference` for debug builds. Doing so has the advantage that the `TypeInference` type is smaller of which we'll have many. However, a `ScopeId` is a `u32`... so it shouldn't matter that much and it avoids storing the `scope` both on `TypeInference` and `TypeInferenceBuilder`

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-10-15 15:22_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-15 15:22_

---

_Label `red-knot` added by @MichaReiser on 2024-10-15 15:22_

---

_Comment by @MichaReiser on 2024-10-15 15:55_

Okay I think I know what the bug is... we infer the parameters in the enclosing scope instead of inferring them in the function's scope. Do we even have to infer the parameters? Can't we just let them be inferred lazily 

---

_Comment by @MichaReiser on 2024-10-15 15:59_

I'm sure someone will tell me why my fix is too simple :laughing: 

My thinking is that we can defer inferring parameters until we need to lookup the type of the parameter. We may need to infer parameters eagerly if we want to check the type annotation but we don't do that today.

---

_Comment by @github-actions[bot] on 2024-10-15 16:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "Add scope assertion to `TypeInference.extend" to "Add scope assertion to `TypeInference.extend`" by @MichaReiser on 2024-10-15 16:17_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-10-15 19:10_

---

_Comment by @dhruvmanila on 2024-10-16 05:34_

I think what the core issue is that the parameters are inferred in either the function's enclosing scope (if type parameters are absent) or the type parameter scope (if type parameters are present) but they're declared in the function scope. 

Now, we can't just move the `infer_parameters` call into `infer_function_body` because that would mean that the annotations are evaluated in the function scope which is incorrect.

One way to fix this would be to use a similar solution as comprehension where we split the inference into annotation and definition. So, the `infer_parameters` would be split into two functions where one would just call `self.infer_optional_expression(parameter.annotation.as_deref());` while the other would just call `self.infer_definition(parameter_with_default);`. So, the following diff which is on top of this PR would still infer the parameter definition and not defer it:

```diff
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index 63e12b98d..faf002346 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -672,6 +672,7 @@ impl<'db> TypeInferenceBuilder<'db> {
     }
 
     fn infer_function_body(&mut self, function: &ast::StmtFunctionDef) {
+        self.infer_parameters_scope(&function.parameters);
         self.infer_body(&function.body);
     }
 
@@ -826,6 +827,27 @@ impl<'db> TypeInferenceBuilder<'db> {
         self.infer_optional_expression(annotation.as_deref());
     }
 
+    fn infer_parameters_scope(&mut self, parameters: &ast::Parameters) {
+        let ast::Parameters {
+            range: _,
+            posonlyargs: _,
+            args: _,
+            vararg,
+            kwonlyargs: _,
+            kwarg,
+        } = parameters;
+
+        for param_with_default in parameters.iter_non_variadic_params() {
+            self.infer_definition(param_with_default);
+        }
+        if let Some(vararg) = vararg.as_deref() {
+            self.infer_definition(vararg);
+        }
+        if let Some(kwarg) = kwarg.as_deref() {
+            self.infer_definition(kwarg);
+        }
+    }
+
     fn infer_parameter_with_default_definition(
         &mut self,
         parameter_with_default: &ast::ParameterWithDefault,
@@ -2111,6 +2133,9 @@ impl<'db> TypeInferenceBuilder<'db> {
     }
 
     fn infer_lambda_body(&mut self, lambda_expression: &ast::ExprLambda) {
+        if let Some(parameters) = lambda_expression.parameters.as_deref() {
+            self.infer_parameters_scope(parameters);
+        }
         self.infer_expression(&lambda_expression.body);
     }
 
```

---

_Comment by @MichaReiser on 2024-10-16 06:19_

@dhruvmanila that makes sense and thanks for taking the type to proposing a fix. 

What's unclear to me why we have to infer the parameters today. The tests and benchmarks indicate that the code is useless because removing the inference code doesn't regress any test nor does it break the benchmarks. I think this is because we still infer the types of parameters, but we now do it lazily when the parameter is first referenced in the function's body instead of eagerly when declaring the function. I do think we'll want to eagerly visit the function parameters once we add a check that the default value is assignable to the annotated type. However, it's unclear how that would fit into your proposed fix, because the default value is inferred earlier (and in the outer scope). 

That's why my preference is to land the PR and remove parameter inference and re-add eager parameter visitation when we need it (and as lazily as possible because codspeed suggest that doing it lazly is a 1% perf improvement)

---

_Comment by @dhruvmanila on 2024-10-16 08:33_

I see your reasoning and I think that's fine. Maybe we could add a one line comment where the `infer_definition` is removed stating that it's being done lazily when it's used in the function body.

---

_Comment by @MichaReiser on 2024-10-16 14:23_

I'm starting to have second thoughts now that I was supposed to write that comment. Maybe we do need the `infer_definition` call to trigger the duplicate binding violations. @carljm can you shed some lights on when one is supposed to call `infer_definition` and when it's okay to do so lazily?

---

_Comment by @carljm on 2024-10-16 15:42_

Sorry for delayed review! I think this fix is correct as-is, and I don't even think we need to add a comment, because it is the normal and expected state of affairs that definitions are inferred on-demand, unless we are inferring/checking the entire scope in which they occur. We don't need to infer definitions eagerly for the sake of diagnostics: anytime we care about diagnostics in a file, we should be explicitly checking that file, which will infer all scopes fully.

The Definition corresponding to a function parameter belongs to the function body scope: that's the scope in which the name is defined. So we shouldn't be trying to infer that definition in the outer scope, and we don't need to infer it eagerly at function definition time; we can wait until we are inferring in the function body scope.

It is also true that the annotation expression which will provide the binding and declaration type for the parameter Definition must be evaluated in the outer scope! So it is correct that (even after this PR) we explicitly do infer types for the annotations in the outer scope, at function definition time, via `infer_parameters`. (If we didn't do this, I expect that our coverage test would fail with missing expression type for the parameter annotation.)

Once we fix the TODO to actually infer the right type for a parameter Definition (instead of just inferring `Type::Todo` like we do today), `infer_parameter_definition` will need to look up the type for the annotation expression from the outer scope's type inference. But there's already a TODO comment in place clarifying this, so I don't think we need to do anything about that in this PR.

Thanks for the PR and figuring out the fix!

---

_@carljm approved on 2024-10-16 15:43_

---

_Merged by @carljm on 2024-10-16 15:44_

---

_Closed by @carljm on 2024-10-16 15:44_

---

_Branch deleted on 2024-10-16 15:44_

---
