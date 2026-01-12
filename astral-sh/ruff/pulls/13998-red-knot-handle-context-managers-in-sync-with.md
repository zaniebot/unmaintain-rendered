```yaml
number: 13998
title: "[red-knot] Handle context managers in (sync) with statements"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/sync-with-items
created_at: 2024-10-30T13:41:44Z
updated_at: 2024-10-31T15:10:55Z
url: https://github.com/astral-sh/ruff/pull/13998
synced_at: 2026-01-12T15:55:46Z
```

# [red-knot] Handle context managers in (sync) with statements

---

_@MichaReiser_

## Summary

This PR implements, hopefully correct, inference for the target types of with items (sync with statements only). 

The context expression specifies a context manager and the target type must be inferred to the return type of the context manager's `__enter__` method.
That's what this PR implements. 

The ad-hoc handling of protocols is awkward and leads to a lot of repetitive code but I decided not to spent too much time on it because
asserting that the context expression correctly implements the context manager protocol is going to change with proper protocol support. 

## Test Plan

Added tests


---

_Review requested from @carljm by @MichaReiser on 2024-10-30 13:41_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-30 13:41_

---

_Label `red-knot` added by @MichaReiser on 2024-10-30 13:41_

---

_Comment by @github-actions[bot] on 2024-10-30 13:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1014 on 2024-10-30 14:57_

nice catch :-)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1008 on 2024-10-30 14:59_

`async with` statements use a different pair of methods entirely: `__aenter__` and `__aexit__` rather than `__enter__` and `__exit__`

```suggestion
        // TODO: Handle `async with` statements. For `async with` statements,
        // `__aenter__` and `__aexit__` coroutine methods are used (which must be awaited),
        // rather than synchronous `__enter__` and `__aexit__` methods
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1064 on 2024-10-30 15:04_

I don't actually know if simplification here is possible because simply checking if `context_manager_ty` is a subtype of `AbstractContextManager` won't give us the return type of the `__enter__` method. I think we do actually need to manually call the `__enter__` method somehow, and once we've done that we then _have_ to manually consider the case where `__enter__` isn't actually callable... it's hard for me to see what would actually be simpler once we have generalised support for `typing.Protocol`

Well, one thing we could do is to just check that the `__exit__` method is consistent with the `__exit__` method on `AbstractContextManager`, since `__exit__` method definitions can get pretty darn complicated. But we're not checking the call signature of `__exit__` at all right now, so this would be "fending off future complexity" rather than reducing any of the complexity that's already here

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1031 on 2024-10-30 15:15_

PEPs are historical documents that record the rationale and discussion around a change that was made to the language. They are not guaranteed to be up-to-date; it's better to link to the language specification where possible, which is living documentation that we work to keep up-to-date. Here I'd link to:

- https://docs.python.org/3/library/stdtypes.html#typecontextmanager
- https://docs.python.org/3/reference/datamodel.html#context-managers

(Don't ask me why this information is split over two separate pages!)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1054 on 2024-10-30 15:16_

The detail in these error messages is fantastic! Long-term, I'm not sure we'll want all this information in one long sentence like this, but I think this is fine for now

---

_@AlexWaygood had review dismissed on 2024-10-30 15:21_

Nice!

I'm not sure this properly handles the case where e.g. `__enter__` might or might not be defined, e.g.

```py
def coinflip() -> bool:
    return True

class Manager:
    if coinflip():
        def __enter__(self) -> Target:
            return Target()

    def __exit__(self, *args): ...
```

I'm also not sure it's essential that we tackle that now for an initial implementation, but it could be worth at least adding a TODO around it

---

_@MichaReiser reviewed on 2024-10-30 15:50_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1031 on 2024-10-30 15:50_

Thanks. The PEP was the only resource I could find that is specific about the terminology. Your linked documents use that terminology but they never explain it. That's why I prefer keeping the link to the PEP

---

_@MichaReiser reviewed on 2024-10-30 15:50_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1054 on 2024-10-30 15:50_

Agree

---

_@MichaReiser reviewed on 2024-10-30 15:51_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1064 on 2024-10-30 15:51_

I think what would become reusable is testing if the context manager has the right shape. Once we know that, we can call `enter` (which should never fail at this point)

---

_Comment by @MichaReiser on 2024-10-30 15:51_

> I'm not sure this properly handles the case where e.g. __enter__ might or might not be defined, e.g.

Lol, python allows if statements in class bodies

---

_@AlexWaygood reviewed on 2024-10-30 16:13_

Oh, it's okay to skip implementing the logic for async `with` statements yet, and just infer `Todo` for them. But this PR introduces false positives for `async with` statements, and I don't think that's okay.

On this snippet:

```py
class Foo:
    async def __aenter__(self):
        return "foo"

    async def __aexit__(self, *args):
        pass

async def foo():
    async with Foo() as x:
        reveal_type(x)
```

red-knot reports with your PR branch:

```
ERROR /Users/alexw/dev/experiment/foo.py:9:16: Object of type Foo cannot be used with `with` because it doesn't implement `__enter__` and `__exit__`
ERROR /Users/alexw/dev/experiment/foo.py:10:9: `reveal_type` used without importing it; this is allowed for debugging convenience but will fail at runtime
ERROR /Users/alexw/dev/experiment/foo.py:10:9: Revealed type is `Unknown`
```

I think you might need to do something like this to avoid adding false positives for `async with` statements in this PR:

<details>

```diff
--- a/crates/red_knot_python_semantic/src/semantic_index/builder.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/builder.rs
@@ -734,12 +734,12 @@ where
                     self.flow_merge(break_state);
                 }
             }
-            ast::Stmt::With(ast::StmtWith { items, body, .. }) => {
+            ast::Stmt::With(ast::StmtWith { items, body, is_async, .. }) => {
                 for item in items {
                     self.visit_expr(&item.context_expr);
                     if let Some(optional_vars) = item.optional_vars.as_deref() {
                         self.add_standalone_expression(&item.context_expr);
-                        self.push_assignment(item.into());
+                        self.push_assignment(CurrentAssignment::WithItem { is_async: *is_async, item });
                         self.visit_expr(optional_vars);
                         self.pop_assignment();
                     }
@@ -1011,11 +1011,12 @@ where
                                 },
                             );
                         }
-                        Some(CurrentAssignment::WithItem(with_item)) => {
+                        Some(CurrentAssignment::WithItem{is_async, item}) => {
                             self.add_definition(
                                 symbol,
                                 WithItemDefinitionNodeRef {
-                                    node: with_item,
+                                    is_async,
+                                    node: item,
                                     target: name_node,
                                 },
                             );
@@ -1232,7 +1233,10 @@ enum CurrentAssignment<'a> {
         node: &'a ast::Comprehension,
         first: bool,
     },
-    WithItem(&'a ast::WithItem),
+    WithItem {
+        is_async: bool,
+        item: &'a ast::WithItem,
+    },
 }
 
 impl<'a> From<&'a ast::StmtAnnAssign> for CurrentAssignment<'a> {
@@ -1259,12 +1263,6 @@ impl<'a> From<&'a ast::ExprNamed> for CurrentAssignment<'a> {
     }
 }
 
-impl<'a> From<&'a ast::WithItem> for CurrentAssignment<'a> {
-    fn from(value: &'a ast::WithItem) -> Self {
-        Self::WithItem(value)
-    }
-}
-
 struct CurrentMatchCase<'a> {
     /// The pattern that's part of the current match case.
     pattern: &'a ast::Pattern,
diff --git a/crates/red_knot_python_semantic/src/semantic_index/definition.rs b/crates/red_knot_python_semantic/src/semantic_index/definition.rs
index 4da4c4e6a..f1826d1de 100644
--- a/crates/red_knot_python_semantic/src/semantic_index/definition.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/definition.rs
@@ -174,6 +174,7 @@ pub(crate) struct AssignmentDefinitionNodeRef<'a> {
 
 #[derive(Copy, Clone, Debug)]
 pub(crate) struct WithItemDefinitionNodeRef<'a> {
+    pub(crate) is_async: bool,
     pub(crate) node: &'a ast::WithItem,
     pub(crate) target: &'a ast::ExprName,
 }
@@ -277,8 +278,9 @@ impl DefinitionNodeRef<'_> {
                     DefinitionKind::ParameterWithDefault(AstNodeRef::new(parsed, parameter))
                 }
             },
-            DefinitionNodeRef::WithItem(WithItemDefinitionNodeRef { node, target }) => {
+            DefinitionNodeRef::WithItem(WithItemDefinitionNodeRef { is_async, node, target }) => {
                 DefinitionKind::WithItem(WithItemDefinitionKind {
+                    is_async,
                     node: AstNodeRef::new(parsed.clone(), node),
                     target: AstNodeRef::new(parsed, target),
                 })
@@ -329,7 +331,7 @@ impl DefinitionNodeRef<'_> {
                 ast::AnyParameterRef::Variadic(parameter) => parameter.into(),
                 ast::AnyParameterRef::NonVariadic(parameter) => parameter.into(),
             },
-            Self::WithItem(WithItemDefinitionNodeRef { node: _, target }) => target.into(),
+            Self::WithItem(WithItemDefinitionNodeRef { is_async: _, node: _, target }) => target.into(),
             Self::MatchPattern(MatchPatternDefinitionNodeRef { identifier, .. }) => {
                 identifier.into()
             }
@@ -532,11 +534,16 @@ pub enum AssignmentKind {
 
 #[derive(Clone, Debug)]
 pub struct WithItemDefinitionKind {
+    is_async: bool,
     node: AstNodeRef<ast::WithItem>,
     target: AstNodeRef<ast::ExprName>,
 }
 
 impl WithItemDefinitionKind {
+    pub(crate) fn is_async(&self) -> bool {
+        self.is_async
+    }
+
     pub(crate) fn node(&self) -> &ast::WithItem {
         self.node.node()
     }
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index 9ab4cb65b..6550f2ee2 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -476,7 +476,12 @@ impl<'db> TypeInferenceBuilder<'db> {
                 self.infer_parameter_with_default_definition(parameter_with_default, definition);
             }
             DefinitionKind::WithItem(with_item) => {
-                self.infer_with_item_definition(with_item.target(), with_item.node(), definition);
+                self.infer_with_item_definition(
+                    with_item.is_async(),
+                    with_item.target(),
+                    with_item.node(),
+                    definition,
+                );
             }
             DefinitionKind::MatchPattern(match_pattern) => {
                 self.infer_match_pattern_definition(
@@ -999,7 +1004,7 @@ impl<'db> TypeInferenceBuilder<'db> {
     fn infer_with_statement(&mut self, with_statement: &ast::StmtWith) {
         let ast::StmtWith {
             range: _,
-            is_async: _,
+            is_async,
             items,
             body,
         } = with_statement;
@@ -1017,7 +1022,9 @@ impl<'db> TypeInferenceBuilder<'db> {
                 // Call into the context expression inference to validate that it evaluates
                 // to a valid context manager.
                 let context_expression_ty = self.infer_expression(&item.context_expr);
-                self.infer_context_expression(&item.context_expr, context_expression_ty);
+                if !is_async {
+                    self.infer_context_expression(&item.context_expr, context_expression_ty);
+                }
                 self.infer_optional_expression(target);
             }
         }
@@ -1027,6 +1034,7 @@ impl<'db> TypeInferenceBuilder<'db> {
 
     fn infer_with_item_definition(
         &mut self,
+        is_async: bool,
         target: &ast::ExprName,
         with_item: &ast::WithItem,
         definition: Definition<'db>,
@@ -1034,10 +1042,14 @@ impl<'db> TypeInferenceBuilder<'db> {
         let context_expr = self.index.expression(&with_item.context_expr);
         self.extend(infer_expression_types(self.db, context_expr));
 
-        let target_ty = self.infer_context_expression(
-            &with_item.context_expr,
-            self.expression_ty(&with_item.context_expr),
-        );
+        let target_ty = if is_async {
+            Type::Todo
+        } else {
+            self.infer_context_expression(
+                &with_item.context_expr,
+                self.expression_ty(&with_item.context_expr),
+            )
+        };
 
         self.types
             .expressions
```

</details>

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/declaration/with.md`:1 on 2024-10-30 17:16_

Nit about this title, and the location of this file: with statements create Definitions, which are Bindings, but are not Declarations. (A declaration is only an annotated assignment, or an annotated function parameter, or a function or class definition.) So I think this title should be just `With statements`.

Regarding file location, we don't have a general directory for all kinds of definitions/bindings, so I think `with/` could be a top-level directory if/when we have multiple files to put in there; for now a top level `with.md` would be fine, I think.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/declaration/with.md`:49 on 2024-10-30 17:17_

The diagnostic messages here and below are excellent! Super clear and informative.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1012 on 2024-10-30 17:24_

While we're editing this comment anyway, this part isn't accurate anymore either. I think it could be edited as below. Arguably that doesn't give much information that isn't obvious, but I think it's not 100% obvious that `Type::call` means "what happens if I call an object of this type", so I'd probably still favor keeping the short doc comment.

We could also do a much longer doc comment here or elsewhere that talks about how to actually correctly use `CallOutcome`, but that's definitely out of scope for this PR :) 

```suggestion
    /// Return the outcome of calling an object of this type.
```

---

_Comment by @MichaReiser on 2024-10-30 17:41_

> I'm not sure this properly handles the case where e.g. __enter__ might or might not be defined, e.g.

nice catch. I added a test with a todo. I want to wait with handling this until we removed unbound.

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-30 17:41_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1064 on 2024-10-30 17:42_

I think maybe what Alex is implying is that it could be kind of wasteful/unnecessary to check `__enter__` against the protocol, when simply calling it and seeing if the call succeeds or fails is actually sufficient. But this is a question that we can leave for later, I think the comment is reasonable as-is. We can always change our mind when actually implementing a TODO if it turns out not to make sense to to do it that way.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/declaration/async_with.md`:6 on 2024-10-30 17:45_

nit: quite long lines here. Also, we haven't renamed it to `Knot` _yet_ ;)

```suggestion
The type of the target variable in a `with` statement should be the return type
from the context manager's `__aenter__` method. However, we don't support `async with`
statements yet. This test asserts that it doesn't emit any context manager-related errors.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/declaration/with.md`:5 on 2024-10-30 17:45_

```suggestion
The type of the target variable in a `with` statement is the return type
from the context manager's `__enter__` method.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1064 on 2024-10-30 17:48_

whatever the case, this is a very long line ;)

```suggestion
        // TODO: The checks here should be simplified to checking
        // if context manager implements the `contextlib.AbstractContextManager` protocol.
```

---

_@AlexWaygood approved on 2024-10-30 17:48_

Thanks! This LGTM now.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1116 on 2024-10-30 17:50_

The return type here will matter to us later, because it will determine whether exceptions are suppressed by this `with` block. We could add a TODO for that but I don't think it's important that we do.

---

_@carljm approved on 2024-10-30 17:50_

This is really well done! Super clear, thorough, and well-researched.

---

_@MichaReiser reviewed on 2024-10-31 07:31_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/declaration/async_with.md`:6 on 2024-10-31 07:31_

I'm open to enabling markdown formatting but I otherwise prefer not to reflow text manually. This does not affect how the text is rendered

---

_Merged by @MichaReiser on 2024-10-31 08:18_

---

_Closed by @MichaReiser on 2024-10-31 08:18_

---

_Branch deleted on 2024-10-31 08:18_

---

_@AlexWaygood reviewed on 2024-10-31 08:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/declaration/async_with.md`:6 on 2024-10-31 08:20_

> This does not affect how the text is rendered

I know. My concern is more the ability to read and edit the code on small screens. But I agree it's not the most important issue in the world, just a nit.

---

_@carljm reviewed on 2024-10-31 15:10_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/declaration/async_with.md`:6 on 2024-10-31 15:10_

https://github.com/astral-sh/ruff/pull/14020

---
