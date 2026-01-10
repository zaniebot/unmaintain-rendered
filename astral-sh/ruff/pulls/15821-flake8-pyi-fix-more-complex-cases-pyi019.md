```yaml
number: 15821
title: "[`flake8-pyi`] Fix more complex cases (`PYI019`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: PYI019
created_at: 2025-01-30T00:09:47Z
updated_at: 2025-02-02T19:15:34Z
url: https://github.com/astral-sh/ruff/pull/15821
synced_at: 2026-01-10T19:57:22Z
```

# [`flake8-pyi`] Fix more complex cases (`PYI019`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-30 00:09_

## Summary

Resolves #15798.

`PYI019` is now a binding-based rule. All references to the type variable will now be replaced correctly. As a result, the fix is now safe in most cases; when that is not possible, its applicability remains display-only. Additionally, for a safe fix, comments within the fix ranges will cause it to be marked as unsafe.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-01-30 00:09_

---

_Comment by @github-actions[bot] on 2025-01-30 00:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `fixes` added by @dylwil3 on 2025-01-30 13:59_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-01-31 11:56_

---

_Comment by @AlexWaygood on 2025-01-31 12:40_

I'd like for us to fix existing bugs such as https://github.com/astral-sh/ruff/issues/15849 first before extending this rule further (edit: see https://github.com/astral-sh/ruff/pull/15851, and https://github.com/astral-sh/ruff/pull/15853)

---

_Comment by @AlexWaygood on 2025-01-31 15:07_

> Previously, there was also a small bug that would cause a PEP 695 type variable not to be removed correctly were it to be placed at the very end of the type parameter list. This has been fixed as well.

Could you possibly separate this out into an isolated PR? That would make it easier to see which tests are specifically for this bugfix

---

_Comment by @InSyncWithFoo on 2025-01-31 16:19_

@AlexWaygood I submitted #15854. As it happens, this PR overlaps with #15853 as well. I'll rebase this once you give the heads-up.

---

_Comment by @AlexWaygood on 2025-01-31 16:55_

> I'll rebase this once you give the heads-up.

Okay, I'm done -- rebase at your leisure! Thanks :-)

---

_Comment by @InSyncWithFoo on 2025-02-01 04:00_

@AlexWaygood This PR now introduces a regression:

```python
def shadowed_type():
    type = 1
    class A:
        @classmethod
        def m[S](cls: type[S]) -> S: ...  # error
```

This is due to `semantic.match_builtin_expr()` searching for `type` in the "current" scope (via `self.scope_id` in `.lookup_symbol()`) rather than the scope of the method.

I'm not sure how to fix this. `.simulate_runtime_load_at_location_in_scope()` returns a binding, but a `::Builtin` binding does not have any information as to what symbol it represents. Not to mention, that method expects a string, so `builtins.type` can't be detected correctly. On the other hand, `.resolve_qualified_name()` does not accept a scope.


---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:108 on 2025-02-01 11:23_

I would do

```rs
    let self_or_cls_annotation = parameters
        .posonlyargs
        .iter()
        .chain(&parameters.args)
        .next()?
        .parameter
        .annotation
        .as_ref()?;
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:333 on 2025-02-01 11:23_

```suggestion
    if let Some(edits) = replace_typevar_usages_with_self(
        typevar,
        &self_symbol_binding,
        replace_references_range,
        checker.semantic(),
    ) {
        other_edits.extend(edits);
```

---

_@AlexWaygood reviewed on 2025-02-01 11:24_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:89 on 2025-02-01 15:41_

nit: not really sure we need all these blank lines; the code here isn't that complex

```suggestion
    let semantic = checker.semantic();
    let current_scope = &semantic.scopes[binding.scope];
    let function_def = binding.statement(semantic)?.as_function_def_stmt()?;
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:308 on 2025-02-01 15:43_

nit: I prefer the existing variable name `import_edit` here. It's what we generally call this variable in other rules, and it's more descriptive about what the edit does

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:323 on 2025-02-01 15:43_

```suggestion
        let first_parameter_end = edit.end();
        other_edits.push(edit);
        TextRange::new(first_parameter_end, returns.start())
```

---

_@AlexWaygood reviewed on 2025-02-01 15:43_

---

_Comment by @AlexWaygood on 2025-02-01 16:04_

> @AlexWaygood This PR now introduces a regression:

I think you can do something like this:

```diff
diff --git a/crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs b/crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs
index 4d30fd3eb..7128fddb1 100644
--- a/crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs
+++ b/crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs
@@ -8,7 +8,7 @@ use ruff_python_ast::{
 };
 use ruff_python_semantic::analyze::function_type::{self, FunctionType};
 use ruff_python_semantic::analyze::visibility::{is_abstract, is_overload};
-use ruff_python_semantic::{Binding, BindingKind, SemanticModel};
+use ruff_python_semantic::{Binding, BindingKind, ScopeId, SemanticModel};
 use ruff_text_size::{Ranged, TextRange, TextSize};
 
 use crate::checkers::ast::Checker;
@@ -138,7 +138,7 @@ pub(crate) fn custom_type_var_return_type(
         }),
     };
 
-    if !method.uses_custom_var(semantic) {
+    if !method.uses_custom_var(semantic, binding.scope) {
         return None;
     }
 
@@ -162,9 +162,9 @@ enum Method<'a> {
 }
 
 impl Method<'_> {
-    fn uses_custom_var(&self, semantic: &SemanticModel) -> bool {
+    fn uses_custom_var(&self, semantic: &SemanticModel, scope: ScopeId) -> bool {
         match self {
-            Self::Class(class_method) => class_method.uses_custom_var(semantic),
+            Self::Class(class_method) => class_method.uses_custom_var(semantic, scope),
             Self::Instance(instance_method) => instance_method.uses_custom_var(),
         }
     }
@@ -180,7 +180,7 @@ struct ClassMethod<'a> {
 impl ClassMethod<'_> {
     /// Returns `true` if the class method is annotated with
     /// a custom `TypeVar` that is likely private.
-    fn uses_custom_var(&self, semantic: &SemanticModel) -> bool {
+    fn uses_custom_var(&self, semantic: &SemanticModel, scope: ScopeId) -> bool {
         let Expr::Subscript(ast::ExprSubscript {
             value: cls_annotation_value,
             slice: cls_annotation_typevar,
@@ -196,7 +196,15 @@ impl ClassMethod<'_> {
 
         let cls_annotation_typevar = &cls_annotation_typevar.id;
 
-        if !semantic.match_builtin_expr(cls_annotation_value, "type") {
+        let Expr::Name(ExprName { id, ..}) = &**cls_annotation_value else {
+            return false;
+        };
+
+        if id != "type" {
+            return false;
+        }
+
+        if !semantic.has_builtin_binding_in_scope("type", scope) {
             return false;
         }
 
@@ -206,7 +214,10 @@ impl ClassMethod<'_> {
                 let Expr::Name(return_annotation_typevar) = &**slice else {
                     return false;
                 };
-                if !semantic.match_builtin_expr(value, "type") {
+                let Expr::Name(ExprName { id, ..}) = &**value else {
+                    return false;
+                };
+                if id != "type" {
                     return false;
                 }
                 &return_annotation_typevar.id
diff --git a/crates/ruff_python_semantic/src/model.rs b/crates/ruff_python_semantic/src/model.rs
index 4ef240e23..825ee3256 100644
--- a/crates/ruff_python_semantic/src/model.rs
+++ b/crates/ruff_python_semantic/src/model.rs
@@ -265,13 +265,22 @@ impl<'a> SemanticModel<'a> {
         self.shadowed_bindings.get(&binding_id).copied()
     }
 
-    /// Return `true` if `member` is bound as a builtin.
+    /// Return `true` if `member` is bound as a builtin *in the scope we are currently visiting*.
     ///
     /// Note that a "builtin binding" does *not* include explicit lookups via the `builtins`
     /// module, e.g. `import builtins; builtins.open`. It *only* includes the bindings
     /// that are pre-populated in Python's global scope before any imports have taken place.
     pub fn has_builtin_binding(&self, member: &str) -> bool {
-        self.lookup_symbol(member)
+        self.has_builtin_binding_in_scope(member, self.scope_id)
+    }
+
+    /// Return `true` if `member` is bound as a builtin *in a given scope*.
+    ///
+    /// Note that a "builtin binding" does *not* include explicit lookups via the `builtins`
+    /// module, e.g. `import builtins; builtins.open`. It *only* includes the bindings
+    /// that are pre-populated in Python's global scope before any imports have taken place.
+    pub fn has_builtin_binding_in_scope(&self, member: &str, scope_id: ScopeId) -> bool {
+        self.lookup_symbol_in_scope(member, scope_id, false)
             .map(|binding_id| &self.bindings[binding_id])
             .is_some_and(|binding| binding.kind.is_builtin())
     }
```

It will still mean that the PR introduces a small regression. We will no longer emit the diagnostic on code like this, where the fully qualified `builtins.type` is used in the annotation of the first parameter of a classmethod:

```py
import builtins

class Foo:
    @classmethod
    def m[S](cls: builtins.type[S]) -> builtins.type[S]: ...
```

But false negatives are much better than false positives. And I think the small regression of some edge-case false negatives is hugely outweighed by the improvements you're introducing in this PR!

---

_@AlexWaygood reviewed on 2025-02-01 17:44_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:89 on 2025-02-01 17:44_

I think this is redundant considering you have `let function_def = binding.statement(semantic)?.as_function_def_stmt()?;` a few lines below. That already ensures that we won't emit this diagnostic on a `Binding` unless that binding is associated with a function-definition statement

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:103 on 2025-02-02 12:23_

```suggestion
        .next()?
        .annotation()?;
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:386 on 2025-02-02 12:26_

Can you add some docs to this function? It was quite unclear to me at first why it returns an `Option<Vec<Edit>>` rather than just returning an empty `Vec` in the cases where it currently returns `None`. I think it's because if we can't resolve the binding to something in this module, we can't be sure that the binding refers to a private TypeVar. Is that correct?

It would also be good to explain what `editable_range` represents, and why we only replace references to the TypeVar if the references lie within that range

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:277 on 2025-02-02 12:33_

```suggestion
    /// Return `true` if `member` is bound as a builtin *in a given scope*.
```

And you need ot change the sentence on line 268 to add `*in the scope we are currently visiting*` to the end of it.

---

_@AlexWaygood reviewed on 2025-02-02 12:34_

Do the changes in this PR mean that we could also offer the fix for `.py` files as well as stub files? We could potentially iterate over the references to the TypeVar in the body of the function and replace them with `Self`, right?

---

_Comment by @InSyncWithFoo on 2025-02-02 18:00_

> Do the changes in this PR mean that we could also offer the fix for `.py` files as well as stub files?

Very unfortunately, no. This would have problems similar to that of #15862.

---

_Comment by @AlexWaygood on 2025-02-02 18:30_

> Very unfortunately, no. This would have problems similar to that of #15862.

You're worried about references inside stringized type expressions? That doesn't seem like a _great_ reason to restrict the rule to stub files, since stringized type expressions are legal in stub files as well as `.py` files (they're just not encouraged). It would be better to detect if any of the references are inside stringized type expressions, and make the fix display-only if so, but to run the fix on both stub files and `.py` files.

But anyway, let's land this as-is for now. It's a big improvement as it is, and the PR is big enough as it is. I have some followup work locally that I can file as PRs to build on this.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:381 on 2025-02-02 18:31_

```suggestion
/// Returns a series of [`Edit`]s that modify all references to the given `typevar`,
```

---

_@AlexWaygood approved on 2025-02-02 18:33_

Thank you!

---

_Label `preview` added by @AlexWaygood on 2025-02-02 18:33_

---

_Merged by @AlexWaygood on 2025-02-02 18:38_

---

_Closed by @AlexWaygood on 2025-02-02 18:38_

---

_Branch deleted on 2025-02-02 19:04_

---

_Comment by @InSyncWithFoo on 2025-02-02 19:09_

> [...] stringized type expressions are legal in stub files as well as `.py` files [...]

True, but we also have [`PYI010`](https://docs.astral.sh/ruff/rules/non-empty-stub-body/) for non-empty bodies and [`PYI020`](https://docs.astral.sh/ruff/rules/quoted-annotation-in-stub/) for stringified types, both of which are only reported for stubs and always safely fixable.

---

_Comment by @AlexWaygood on 2025-02-02 19:15_

Sure, but users are fickle, and may only have some of the flake8-pyi rules enabled ;)

---
