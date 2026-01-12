```yaml
number: 13267
title: "[red-knot] Add definitions and limited type inference for exception handlers"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/redknot-except-defs
created_at: 2024-09-06T11:52:34Z
updated_at: 2024-09-09T11:41:21Z
url: https://github.com/astral-sh/ruff/pull/13267
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] Add definitions and limited type inference for exception handlers

---

_@AlexWaygood_

## Summary

This PR adds definitions, type inference and type-checking for exception handlers to red-knot.

In the following snippet, we now recognise `e` and `f` as being a validly defined symbols:

```py
try:
    x
except NameError as e:
    print(e)
except (TypeError, AttributeError) as f:
    print(f)
```

We also now infer the type of `e` as being `Type::Instance(builtins.NameError)`, and the type of `f` as being `Type::Instance(builtins.TypeError) | Type::Instance(builtins.AttributeError)`. We also check all `except` blocks to validate that exception handlers are only applied to `BaseException` subclasses or tuples of `BaseException` subclasses. I.e., we'll catch invalid `except` blocks such as the following:

```pycon
>>> try:
...     x
... except NameError():  # instance of `NameError` caught, not `NameError` itself
...     pass
... 
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
NameError: name 'x' is not defined

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<stdin>", line 3, in <module>
TypeError: catching classes that do not inherit from BaseException is not allowed
>>> try:
...     x
... except (NameError, (TypeError, AttributeError)):  # Nested tuples aren't allowed
...     pass
... 
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
NameError: name 'x' is not defined

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<stdin>", line 3, in <module>
TypeError: catching classes that do not inherit from BaseException is not allowed
```

## Outstanding issues

Exception-handler bindings are somewhat unique. For example, in this snippet, the `e` variable does not persist after the `except` block has finished:

```pycon
>>> try:
...     x
... except NameError as e:
...     pass
... 
>>> e
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'e' is not defined
```

But in _this_ snippet, it does:

```pycon
>>> try:
...     x
... except NameError as f:
...     e = f
... 
>>> e
NameError("name 'x' is not defined")
```

This strange inconsistency is because `except` handlers don't introduce a new scope for their variables; instead, the bound variable is literally `del`'d by the interpreter after the `except` block ends (to clean up probably-undesirable reference cycles). So, any variable that was _not_ initially bound by the except handler will persist beyond the `except` block -- but these specific definitions will not.

In fact -- any definitions that override the name bound by the except handler are also `del`'d! E.g.

```pycon
>>> try:
...     x
... except NameError as e:
...     print("Got here!")
...     e = 42
...     f = 56
... 
Got here!
>>> f
56
>>> e
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'e' is not defined
```

I don't know how we want to model this, so this PR doesn't attempt to do so. My proposal is that we open a followup issue after this to explore ways of modelling this.

Other outstanding issues that this PR doesn't yet implement:
- control flow for `except` blocks
- Currently the logic only checks that the caught exceptions are a class or tuple of classes. But we should also check whether any caught classes are actually subclasses of `BaseException`.

## Test Plan

- `cargo test -p red_knot_python_semantic`
- `cargo test -p red_knot_workspace`
- `bench -p ruff_benchmark -- red_knot`


---

_Label `red-knot` added by @AlexWaygood on 2024-09-06 11:52_

---

_Review requested from @carljm by @AlexWaygood on 2024-09-06 11:52_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-06 11:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:227 on 2024-09-06 11:54_

An `ast::Identifier` may be an AST node, but it definitely isn't an expression, so I'm not sure if this is really correct. `except` handlers don't use `ast::Name` nodes for the symbols they bound, however, so I'm not sure how to do this otherwise.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:767 on 2024-09-06 11:56_

It seems silly to collect the invalid nodes into a `Vec<>` only to immediately iterate over them, but I couldn't make the borrow checker happy otherwise (it was complaining about immutable and mutable borrows in the same scope).

---

_@AlexWaygood reviewed on 2024-09-06 11:56_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:767 on 2024-09-06 12:01_

Yeah that's simply not possible because `add_diagnostic` takes `&mut self` and `Iterator::next` requires a `&self` because of `self.infer_handled_exception_types`. 

---

_@MichaReiser reviewed on 2024-09-06 12:01_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:825 on 2024-09-06 12:03_

```suggestion
    fn infer_except_handler_definition(
        &mut self,
        symbol_name: &ast::Identifier,
        handled_exceptions: &'db ast::Expr,
        definition: Definition<'db>,
    ) {
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:845 on 2024-09-06 12:04_

We can simplify the lifetimes here because all AST nodes have the lifetime `'db`. 
```suggestion
    fn infer_handled_exception_types(
        &self,
        handled_exceptions: &'db ast::Expr,
    ) -> ExceptionHandlerTypeIterator<'_, 'db> {
        match handled_exceptions {
            ast::Expr::Tuple(multiple_exceptions) => ExceptionHandlerTypeIterator::Multiple {
                exceptions: multiple_exceptions.into_iter(),
                inference_builder: self,
            },
            single_exception => ExceptionHandlerTypeIterator::Single {
                exception: Some(single_exception),
                inference_builder: self,
            },
        }
    }
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2472 on 2024-09-06 12:06_

```suggestion
enum ExceptHandlerType<'db> {
    Valid {
        exception_ty: Type<'db>,
    },
    Invalid {
        node: &'db ast::Expr,
        node_ty: Type<'db>,
    },
}
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2492 on 2024-09-06 12:06_

```suggestion
enum ExceptionHandlerTypeIterator<'b, 'db> {
    Single {
        exception: Option<&'db ast::Expr>,
        inference_builder: &'b TypeInferenceBuilder<'db>,
    },
    Multiple {
        exceptions: std::slice::Iter<'db, ast::Expr>,
        inference_builder: &'b TypeInferenceBuilder<'db>,
    },
}
```



---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:831 on 2024-09-06 12:13_

You could get away without introducing your own iterator type by using a slice iterator instead

```rust
    fn infer_handled_exception_types(
        &self,
        handled_exceptions: &'db ast::Expr,
    ) -> impl Iterator<Item = ExceptHandlerType<'db>> + '_ {
        let exceptions = match handled_exceptions {
            ast::Expr::Tuple(multiple_exceptions) => multiple_exceptions.elts.as_slice(),
            single_exception => std::slice::from_ref(single_exception),
        };

        exceptions.into_iter().map(move |exception| {
            let node_ty = self
                .types
                .expression_ty(exception.scoped_ast_id(self.db, self.scope));

            let ty = match node_ty {
                Type::Class(exception_class) => ExceptHandlerType::Valid {
                    exception_ty: Type::Instance(exception_class),
                },
                Type::Any | Type::Unknown => ExceptHandlerType::Valid {
                    exception_ty: node_ty,
                },
                invalid_ty => ExceptHandlerType::Invalid {
                    node: exception,
                    node_ty: invalid_ty,
                },
            };

            ty
        })
    }
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:896 on 2024-09-06 12:15_

What's the reason that we need to treat the name as an expression?

---

_@MichaReiser reviewed on 2024-09-06 12:15_

---

_@AlexWaygood reviewed on 2024-09-06 12:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:831 on 2024-09-06 12:17_

Thanks, TIL!

---

_Comment by @github-actions[bot] on 2024-09-06 12:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-09-06 12:24_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:831 on 2024-09-06 12:24_

`std::slice::from_ref` is awesome :)

---

_@AlexWaygood reviewed on 2024-09-06 12:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:896 on 2024-09-06 12:52_

This is the same issue I highlighted in https://github.com/astral-sh/ruff/pull/13267/files#r1746998476, more or less. The fundamental issue is that all our infrastructure currently assumes that the "target" of a definition will be an `ast::Expr` node, but that isn't true for except-handler definitions.

`HasScopedId::scoped_ast_id()` needs to be called in `infer_except_handler_definition()` on an `ast::Identifier` node in order to store the type associated with this definition. This means that `HasScopedId::scoped_ast_id()` must be implemented for `ast::Identifier`; but `HasScopedId::scoped_ast_id()` won't work for the `ast::Identifier` nodes here unless we also store them in the `AstIds::expressions_map`. Otherwise we end up with this crash:

```
---- types::infer::tests::except_handler stdout ----
thread 'types::infer::tests::except_handler' panicked at crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs:37:29:
no entry found for key
```

I suppose we could store the except-handler IDs in a different map? `AstIds::identifiers_map`? And look up the `Identifier` node in that map from the `HasScopedId::scoped_ast_id` implementation for `ast::Identifier`? It seems a little wasteful to create a separate `HashMap` just for this purpose, though, since the vast majority of definitions have `Expr` nodes as their targets -- except-handler definitions are somewhat unique here?

---

_@MichaReiser reviewed on 2024-09-06 13:06_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:896 on 2024-09-06 13:06_

What's the reason that we need to store the type of the symbol? It's not an expression. Isn't it enough to just store the type of the definition?

The main issue I see is from a linter API perspective where calling `.ty` on an except handler is not possible but maybe the solution here is that `ExceptHandler` implements `HasTy`?

Edit: Do we need to implement `HasTy` for `ExceptHandler` and explicitly call it in the corpus test?

---

_@AlexWaygood reviewed on 2024-09-06 13:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:896 on 2024-09-06 13:23_

I see what you mean. Sorry, I guess I'm still getting to grips with some of the semantic-index code. I pushed https://github.com/astral-sh/ruff/pull/13267/commits/7b507e8ddc0d859f2a4b4b47bde9c7244fb195d9, and all tests seem to pass, at least for now.

---

_@AlexWaygood reviewed on 2024-09-06 13:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:896 on 2024-09-06 13:35_

> Edit: Do we need to implement `HasTy` for `ExceptHandler` and explicitly call it in the corpus test?

I'm not sure it would be correct for `ExceptHandler` to implement `HasTy`. What would be the type of `ExceptHandler`s like these? In the first one, the `name` field is set to `None`; in the second one, `name` and `type_` are both `None`:

```py
try:
    x
except NameError:
    pass

try:
    y
except:
    pass
```

---

_@MichaReiser reviewed on 2024-09-06 13:50_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:896 on 2024-09-06 13:50_

Good point. I think the way it would be done is to call `.ty` on the handled exception expression? Interested to hear what @carljm thinks

---

_Comment by @Skylion007 on 2024-09-06 13:52_

FYI, normal ruff already have some issues modeling deleted functions etc, and this might be related to reducing false positives on those rules as well. I think the current infra just doesn't handle del statements well, period.

---

_Comment by @AlexWaygood on 2024-09-06 13:55_

> FYI, normal ruff already have some issues modeling deleted functions etc, and this might be related to reducing false positives on those rules as well. I think the current infra just doesn't handle del statements well, period.

red-knot uses an entirely different semantic model from Ruff, and we should aim to improve on Ruff's existing semantic model where possible :D especially since existing type checkers like mypy model this behaviour correctly: https://mypy-play.net/?mypy=latest&python=3.12&gist=160e76a009c773676db6c370ffb716de

---

_Comment by @carljm on 2024-09-06 23:54_

> I don't know how we want to model this, so this PR doesn't attempt to do so. My proposal is that we open a followup issue after this to explore ways of modelling this.

I agree that it can be out of scope for this PR. I haven't created an issue for it, but I did add it to the roadmap doc.

I think we should do this at the same time that we add support for the `del` statement, and the implementation should basically be to act as if there is an invisible `del e` at the end of an `except e:` block. (That's literally how it works in bytecode.)

And I don't think `del` support should be too hard, it's just another `Definition`, which assigns `Unbound` to the name.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:894 on 2024-09-07 00:18_

I don't think we should need to add the type as a standalone expression, because there's no unpacking to multiple names here. The unpacking / multiple-target cases are where we need a standalone expression, so that we can query its type directly as a cached Salsa query, and avoid inferring its type multiple times (once for each `Definition` for each target name it can be unpacked to.) In this case, there's no unpacking, so only one Definition can possibly include this expression, so we can just infer its type as part of the Definition without any duplication. (And it's more efficient to avoid standalone expressions and the extra ingredients/queries they imply, when possible.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:896 on 2024-09-07 00:20_

This is something we generally do if we're about to set a `self.current_assignment`, which we aren't doing here. So I don't think we really need this? But it also doesn't hurt anything -- it should still be true that there isn't any other current assignment when we're visiting an except handler.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:902 on 2024-09-07 00:22_

Again, since there's no unpacking, and thus a maximum of one Definition per exception handler, I would probably simplify this to just store a reference to the entire except-handler node as part of the `Definition`. We can get to all the parts of it from there easily, and it makes the `Definition` a bit smaller. It also simplifies this code, because we can just treat the node itself directly as an `Into<DefinitionNodeRef>`, we don't have to construct it explicitly.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:895 on 2024-09-07 00:24_

I think we can have this just once, instead of both here and in the `else`, and put it above, outside the `if let Some(symbol_name) ...`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:177 on 2024-09-07 00:26_

Like I mentioned above, I don't think we need this struct at all; we can instead just have the `DefinitionNodeRef::ExceptHandler` variant directly wrap an `&'a ast::ExceptHandler`, and implement `From<&'a ast::ExceptHandler>` for `DefinitionNodeRef`, more like we do for functions, classes, annassign, etc.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:269 on 2024-09-07 00:34_

Here as well, I think `DefinitionKind::ExceptHandler` can directly wrap an `AstNodeRef<ast::ExceptHandler>`; more like e.g. AnnAssign; we don't need the `ExceptHandlerDefinitionKind` struct at all.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:303 on 2024-09-07 00:36_

It will be fine here for the `DefinitionNodeKey` for an except handler to be a NodeKey for the entire except handler node, because there's no ambiguity; a given except handler can only ever have zero or one `Definition` associated with it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:775 on 2024-09-07 00:41_

Why not emit the diagnostic in `infer_handled_exception_types` and avoid the need for the `filter_map` and the `ExceptHandlerType` enum entirely?

EDIT: on second thought, I'm guessing you did this to avoid double-emitting diagnostics? But we don't want to allow double inference to happen and then use tricks like this to avoid double-emitting diagnostics. Double-inference is something that should never happen, and double-emitting diagnostics is just a sign we need to fix that. The right fix is to defer to `self.infer_definition` as I described above, and then you can just emit the diagnostic in `infer_handled_exception_types` without risking double diagnostics.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:754 on 2024-09-07 00:44_

We need to avoid double-inferring the except handler in the case where someone queries for it as an individual Definition (thus we enter only at `infer_except_handler_definition`), but we also query types for the entire scope (thus arriving here.) That means what we need to do here is check whether this except-handler is one that we would have created a `Definition` for (i.e. does it have a capture name), and if so, all we should do is call `self.infer_definition` on it, which will use the `infer_definition_types` query and then populate those types into our own scope types. If not, then we can just infer types for the parts of the except handler and move on.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:854 on 2024-09-07 00:51_

Really this entire match statement should become a single call to a generalized "is assignable to" helper; a union of exception types is assignable to `BaseException`, so the only question we need to ask here is "is the type assignable to `BaseException`". Support for subtype-of / assignable-to checks is one of the next priority items on our list.

I think it's great that you went ahead and made a start on the validation here, but let's add a TODO referencing generalized assignable-to checking, and then I'm not sure the other comments even need to be TODOs in their own right (since we won't likely make those fixes here), they can just mention what's known-missing from the current code to avoid anyone being confused.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:866 on 2024-09-07 00:54_

This can go away along with the `add_standalone_expression` call in the semantic builder, and just become an `infer_expression` instead.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:869 on 2024-09-07 00:55_

I think once we make some of the other changes mentioned here (e.g. eliminate the need for `ExceptHandlerType`), `infer_handled_exception_types` can probably just directly return the union.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2486 on 2024-09-07 00:57_

Interesting question here whether an invalid exception type should contribute `Unknown` or `Never` to the union. I think it should be `Never` instead. If we don't know the type of one of the exception types, that'll already contribute `Unknown`. If we do know the type, and it's not valid, then that is never going to catch an exception or become part of the type of the target name.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4274 on 2024-09-07 00:58_

Here's where I think we should type `e` as `Never` instead.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4246 on 2024-09-07 00:58_

And here I think we should have just `TypeError` rather than `TypeError | Unknown`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4291 on 2024-09-07 00:59_

If the point of this test is that `foo` doesn't cause a problem, why do we need this clause in this test? It seems like a repeat of the above test.

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:36 on 2024-09-07 00:59_

🎉 

---

_@carljm requested changes on 2024-09-07 01:00_

Awesome! I think there are a few things to tweak/simplify here, but the shape looks great. So sweet to see that bogus "undefined name" error in tomllib go away.

---

_@carljm reviewed on 2024-09-07 05:37_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:896 on 2024-09-07 05:37_

I didn't see this code anymore when I reviewed the current version of the PR, so I think it must have already been removed? I don't actually think we should make the assumption anywhere that assignment targets must be Name nodes; that's not true for imports either.

Regarding `HasTy`, I generally think it's not important that we implement it for all non-expression definition nodes, if it's not self-evident that all nodes of that kind will have a clear and obvious type. I think the main problem here is bare `except` -- if it weren't for that, we _could_ implement `HasTy` for all `ExceptHandler` nodes, and it could be the handled-exceptions union that we get from analyzing the handled-exceptions (the fact that the exception isn't captured in a name doesn't stop us from implementing `HasTy`).

But I also don't think it's _important_ that we implement `HasTy` for except-handler nodes, and since the bare `except` problem is a little thorny, I would probably just not bother.

---

_@AlexWaygood reviewed on 2024-09-07 10:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:894 on 2024-09-07 10:08_

Is this the change you're suggesting for this function?

```diff
diff --git a/crates/red_knot_python_semantic/src/semantic_index/builder.rs b/crates/red_knot_python_semantic/src/semantic_index/builder.rs
index 80210a11c..668ad4b77 100644
--- a/crates/red_knot_python_semantic/src/semantic_index/builder.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/builder.rs
@@ -889,22 +889,18 @@ where
             body,
             range: _,
         }) = except_handler;
-        if let Some(type_) = type_ {
+        if let Some(handled_exceptions) = type_ {
+            self.visit_expr(handled_exceptions);
             if let Some(symbol_name) = name {
-                self.add_standalone_expression(type_);
-                self.visit_expr(type_);
-                debug_assert!(self.current_assignment.is_none());
                 let symbol =
                     self.add_or_update_symbol(symbol_name.id.clone(), SymbolFlags::IS_DEFINED);
                 self.add_definition(
                     symbol,
                     ExceptHandlerDefinitionNodeRef {
                         symbol_name,
-                        handled_exceptions: type_,
+                        handled_exceptions,
                     },
                 );
-            } else {
-                self.visit_expr(type_);
             }
         }
         self.visit_body(body);
```

It causes the tests I added to panic:

```
failures:

---- types::infer::tests::except_handler stdout ----
thread 'types::infer::tests::except_handler' panicked at crates/red_knot_python_semantic/src/semantic_index.rs:207:33:
no entry found for key

---- types::infer::tests::invalid_except_handler stdout ----
thread 'types::infer::tests::invalid_except_handler' panicked at crates/red_knot_python_semantic/src/semantic_index.rs:207:33:
no entry found for key

---- types::infer::tests::unknown_type_in_except_handler_does_not_cause_spurious_diagnostic stdout ----
thread 'types::infer::tests::unknown_type_in_except_handler_does_not_cause_spurious_diagnostic' panicked at crates/red_knot_python_semantic/src/semantic_index.rs:207:33:
no entry found for key


failures:
    types::infer::tests::except_handler
    types::infer::tests::invalid_except_handler
    types::infer::tests::unknown_type_in_except_handler_does_not_cause_spurious_diagnostic

test result: FAILED. 236 passed; 3 failed; 2 ignored; 0 measured; 0 filtered out; finished in 0.18s
```

---

_@AlexWaygood reviewed on 2024-09-07 10:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:902 on 2024-09-07 10:10_

An `ExceptHandler` node only creates a definition if the `type_` and `name` fields of the `ExceptHandler` are both `Some()`. I.e., `ExceptHandler`s like these do not create definitions (for the first one, `name` and `type_` are both `None`; for the second one, `type_` is `ast::Name("NameError")` but `name` is still `None`):

```py
try:
    x
except:
    pass

try:
    x
except NameError:
    pass
```

If we stored a reference to the entire except-handler node here rather than storing references to the symbol name and the handled exceptions (respectively an `Identifier` and an `Expr`), we'd have to do unnecessary type narrowing or unwrapping at later stages. It seems to me like it would be much less type-safe and much less ergonomic.

---

_@AlexWaygood reviewed on 2024-09-07 10:12_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:177 on 2024-09-07 10:12_

Same response as https://github.com/astral-sh/ruff/pull/13267#discussion_r1748035302

---

_@carljm reviewed on 2024-09-07 10:17_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:894 on 2024-09-07 10:17_

Did you also change the code I mentioned in `infer.rs` that currently calls `infer_expression_types` salsa query on this expression, but should instead just use `self.infer_expression`? That call to `infer_expression_types` query  requires this call to `add_standalone_expression` here -- but we don't need either one.

---

_@AlexWaygood reviewed on 2024-09-07 10:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:854 on 2024-09-07 10:18_

Yeah, I realised after sleeping on it that my current approach of distinguishing between a literal tuple in the AST and other nodes is hopelessly naive. You can of course have dynamic tuples in `except` blocks:

```pycon
>>> EXCEPTIONS = (TypeError, AttributeError)
>>> try:
...     object.foo
... except EXCEPTIONS:
...     pass
... 
>>> 
```

So it may be better to just axe all this type-checking until we have the tools to do it properly. There's certainly no way we can check if something is a consistent subtype of `tuple[type[BaseException], ...] | type[BaseException]` right now, and only permitting a literal tuple of exceptions else risks emitting false positives on user code :(

---

_@carljm reviewed on 2024-09-07 10:21_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:902 on 2024-09-07 10:21_

Once the inference code is fixed to avoid double inference, we will unavoidably have to do this type narrowing and unwrapping again once in inference either way, because we will run into the exception handler in scope visiting and have to a) decide if it is a kind of except handler that creates a definition, and b) unwrap the relevant parts of it again. The only difference is whether we have to do the unwrapping in `infer_except_handler` (in your version, so we can find the Definition) or in `infer_except_handler_definition` (if the Definition just stores the whole node). So there is no gain in either type safety or ergonomics. But there is a small loss in efficiency and ergonomics on this side due to the extra wrapping types needed. 

---

_@carljm reviewed on 2024-09-07 10:28_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:894 on 2024-09-07 10:28_

I don't like that this results in a panic instead of somehow being enforced by the type system; I would rather that we can use a salsa query on any expression. But that's a necessary concession to salsa performance issues. And since our base representation is the AST, which we can't mutate, we are left with this fundamental issue that some `ast::Expr` have a matching `Expression` tracked struct we can query on and some do not, and the type system doesn't know the difference. 

---

_Renamed from "[red-knot] Add definitions, type inference and type checking for exception handlers" to "[red-knot] Add definitions and limited type inference for exception handlers" by @AlexWaygood on 2024-09-07 10:45_

---

_@AlexWaygood reviewed on 2024-09-07 14:38_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:894 on 2024-09-07 14:38_

> Did you also change the code I mentioned in `infer.rs` that currently calls `infer_expression_types` salsa query on this expression, but should instead just use `self.infer_expression`? That call to `infer_expression_types` query requires this call to `add_standalone_expression` here -- but we don't need either one.

Yup, making that change at the same time fixed it. Thanks :-)

---

_@AlexWaygood reviewed on 2024-09-07 14:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4274 on 2024-09-07 14:43_

`foo` isn't necessarily an invalid object to be caught by an exception handler, though. We just don't know, because we weren't able to resolve `nonexistent_module` and inspect where it came from. It could be perfectly valid (maybe `nonexistent_module` is a C extension that doesn't have stubs), and we've already emitted an error at the import statement complaining that we couldn't resolve the import.

Inferring `Never` for `e` could create lots of false-positive errors when the `e` variable is actually used, since `Never` has no attributes or methods.

---

_Comment by @codspeed-hq[bot] on 2024-09-07 14:44_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex/redknot-except-defs)

### Merging #13267 will **not alter performance**

<sub>Comparing <code>alex/redknot-except-defs</code> (73eb467) with <code>main</code> (346dbf4)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:902 on 2024-09-07 14:59_

So you want me to do something like this diff? It seems to me like there is clearly a loss of type safety here: I have to add a `.expect()` call in `infer_except_handler_definition()` that's unnecessary in the code as I currently have it:

<details>
<summary>Diff</summary>

```diff
diff --git a/crates/red_knot_python_semantic/src/semantic_index/builder.rs b/crates/red_knot_python_semantic/src/semantic_index/builder.rs
index 668ad4b77..0c00472c3 100644
--- a/crates/red_knot_python_semantic/src/semantic_index/builder.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/builder.rs
@@ -883,12 +883,13 @@ where
     }
 
     fn visit_except_handler(&mut self, except_handler: &'ast ast::ExceptHandler) {
-        let ast::ExceptHandler::ExceptHandler(ast::ExceptHandlerExceptHandler {
+        let ast::ExceptHandler::ExceptHandler(except_handler) = except_handler;
+        let ast::ExceptHandlerExceptHandler {
             name,
             type_,
             body,
             range: _,
-        }) = except_handler;
+        } = except_handler;
         if let Some(handled_exceptions) = type_ {
             self.visit_expr(handled_exceptions);
             if let Some(symbol_name) = name {
@@ -898,7 +899,7 @@ where
                     symbol,
                     ExceptHandlerDefinitionNodeRef {
                         symbol_name,
-                        handled_exceptions,
+                        except_handler,
                     },
                 );
             }
diff --git a/crates/red_knot_python_semantic/src/semantic_index/definition.rs b/crates/red_knot_python_semantic/src/semantic_index/definition.rs
index cce6de9db..aa05fa468 100644
--- a/crates/red_knot_python_semantic/src/semantic_index/definition.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/definition.rs
@@ -172,7 +172,7 @@ pub(crate) struct ComprehensionDefinitionNodeRef<'a> {
 #[derive(Copy, Clone, Debug)]
 pub(crate) struct ExceptHandlerDefinitionNodeRef<'a> {
     pub(crate) symbol_name: &'a ast::Identifier,
-    pub(crate) handled_exceptions: &'a ast::Expr,
+    pub(crate) except_handler: &'a ast::ExceptHandlerExceptHandler,
 }
 
 #[derive(Copy, Clone, Debug)]
@@ -262,11 +262,9 @@ impl DefinitionNodeRef<'_> {
                 index,
             }),
             DefinitionNodeRef::ExceptHandler(ExceptHandlerDefinitionNodeRef {
-                handled_exceptions,
+                except_handler,
                 ..
-            }) => DefinitionKind::ExceptHandler(ExceptHandlerDefinitionKind {
-                handled_exceptions: AstNodeRef::new(parsed, handled_exceptions),
-            }),
+            }) => DefinitionKind::ExceptHandler(AstNodeRef::new(parsed.clone(), except_handler)),
         }
     }
 
@@ -322,18 +320,7 @@ pub enum DefinitionKind {
     ParameterWithDefault(AstNodeRef<ast::ParameterWithDefault>),
     WithItem(WithItemDefinitionKind),
     MatchPattern(MatchPatternDefinitionKind),
-    ExceptHandler(ExceptHandlerDefinitionKind),
-}
-
-#[derive(Clone, Debug)]
-pub struct ExceptHandlerDefinitionKind {
-    handled_exceptions: AstNodeRef<ast::Expr>,
-}
-
-impl ExceptHandlerDefinitionKind {
-    pub(crate) fn handled_exceptions(&self) -> &ast::Expr {
-        self.handled_exceptions.node()
-    }
+    ExceptHandler(AstNodeRef<ast::ExceptHandlerExceptHandler>),
 }
 
 #[derive(Clone, Debug)]
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index 3ddf821ae..486e34fc4 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -426,10 +426,7 @@ impl<'db> TypeInferenceBuilder<'db> {
                 );
             }
             DefinitionKind::ExceptHandler(except_handler_definition) => {
-                self.infer_except_handler_definition(
-                    except_handler_definition.handled_exceptions(),
-                    definition,
-                );
+                self.infer_except_handler_definition(except_handler_definition, definition);
             }
         }
     }
@@ -805,9 +802,12 @@ impl<'db> TypeInferenceBuilder<'db> {
 
     fn infer_except_handler_definition(
         &mut self,
-        handled_exceptions: &'db ast::Expr,
+        except_handler: &'db ast::ExceptHandlerExceptHandler,
         definition: Definition<'db>,
     ) {
+        let handled_exceptions = except_handler.type_.as_deref().expect(
+            "`type_` should always be `Some()` for an exception handler that creates a definition",
+        );
         let node_ty = self.infer_expression(handled_exceptions);
 
         // TODO: anything that's a consistent subtype of
```

</details>

---

_@AlexWaygood reviewed on 2024-09-07 14:59_

---

_@AlexWaygood reviewed on 2024-09-07 16:55_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4274 on 2024-09-07 16:55_

FWIW, both mypy and pyright infer `Any`/`Unknown` in this situation:

- https://mypy-play.net/?mypy=latest&python=3.12&gist=bcdb068154c1fa6bac71335781c392b4
- https://pyright-play.net/?pythonVersion=3.13&strict=true&code=GYJw9gtgBAJmCmBnA%2BgOzAF2fAHgS0QyjwgAcwQjgwwBYAKAYxAE8AuBqLqHB3AY3ikqNKAENEUeB3rcoIeADd4YgDbIMLUvAAU8AJQMgA

---

_@carljm reviewed on 2024-09-08 05:10_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:902 on 2024-09-08 05:10_

Good point, yeah. I think that's good enough reason that the single node we pass through the definition can be the handled-types expression rather than the entire handler. But I still think we have unnecessary complexity that can be removed. Will make it a different comment, since this thread is on "outdated" code in the PR.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:303 on 2024-09-08 05:19_

The idea here was that we don't really need the symbol name here at all, this could be the except handler node, or it could be the handled-exceptions expression, with no loss in correctness.

But, it does cost very little to use the symbol name here, and although it's not required in any way, it's arguably clearer for the definition node key to be the specific name/identifier node that is defined, when that works out. And, if we are going to pass through the handled-exceptions expression instead of the except handler node, then we do need `ExceptHandlerDefinitionNodeRef` either way (whether it contains the symbol name or not), because we can't just implement `From<ast::Expr> for DefinitionNodeRef` -- we wouldn't know what kind of expression we're dealing with. So I think what you currently have makes good sense.

Thanks for bearing with me! My main concern was to have as much parity as feasible between how we handle the different cases, but it turns out every case is a little different! This looks good.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:269 on 2024-09-08 05:20_

I think this remains true, but I think it's still reasonable to prefer to have `ExceptHandlerDefinitionKind`, so as to clarify that the node we are referencing is specifically the handled-exceptions expression.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4274 on 2024-09-08 05:26_

Oh, I think I just commented on the wrong case! `foo` should be `Unknown` here, and I definitely agree that if one of the caught exception types is `Unknown`, that should be valid and should contribute `Unknown` to the resulting union.

Where I think we might want `Never` is for types that we _know_ are not `BaseException` subtypes; the cases where the previous version of this PR emitted a diagnostic. But this version of the PR no longer attempts to detect such cases at all (which I think makes sense), so it's a moot question for this PR.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:821 on 2024-09-08 05:33_

It looks to me like we will still have double-inference of some except handlers in this version of the PR, because now we are entirely missing the code that we need to add in `infer_try_statement`, which needs to check, for each handler, whether it is one that would create a definition (that is, it has a `name`), and if so, call `self.infer_definition(handler.name)` (this is where it matters which node is used as the DefinitionNodeKey; that's what we have to look up the definition by), which will use the per-definition inference query to infer that handler and merge the types back into the scope-level inference.

Otherwise, with the current code in this PR, scope-level inference and definition-level inference will separately double-infer the same except handler (and thus, in the future, potentially double-emit diagnostics.)

Fixing https://github.com/astral-sh/ruff/issues/13168 will allow us to catch these double-inference cases automatically, instead of having to watch for it in code review.

---

_@carljm reviewed on 2024-09-08 05:34_

Thanks for pushing back on preferring the sub-node for type safety!

This version looks good, except that I think it will still do double-inference; we need to add the use of `self.infer_definition(...)` when appropriate in `infer_try_statement`.

---

_@AlexWaygood reviewed on 2024-09-08 06:36_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4274 on 2024-09-08 06:36_

Gotcha! I think we're in agreement. Thanks for clarifying!

---

_Comment by @AlexWaygood on 2024-09-08 22:24_

Hmmm. Coming back to this again, I'm reconsidering the argument I put forward in https://github.com/astral-sh/ruff/pull/13267#discussion_r1748289271... because I'm not sure what our behaviour should be if we come across an except handler with invalid syntax!

Specifically, what should we do for this?

```py
try:
    x
except as e:
    pass
```

Currently my PR does not create a definition for `e` there. But I think that's maybe wrong -- I think we should probably create a definition for `e` and assign it the type `Unknown`. Even though this snippet has invalid syntax, it's reasonably clear that the intent is for `e` to be bound here, I think. And as soon as you concede that an `ExceptHandlerExceptHandler` node with `type_` set to `None` can still create a definition, the argument for having a separate struct rather than just using `AstNodeRef<ExceptHandlerExceptHandler>` falls apart.

WDYT -- does my reasoning make sense w.r.t. what we should do there in the face of invalid syntax?

---

_Comment by @carljm on 2024-09-08 22:31_

Heh, I thought of that particular case, had to double-check myself that it was invalid syntax, and then set it aside as not requiring consideration. I need to form a stronger habit of considering what we'll do for invalid syntax! Thanks for considering this.

I agree that this looks like an argument for simply keeping the entire except-handler node in the Definition, as I'd originally suggested. It means the panic you (reasonably) wanted to avoid instead turns into a "fallback to Unknown."

---

_Comment by @AlexWaygood on 2024-09-08 22:32_

Nice, I'll do that!

---

_@AlexWaygood reviewed on 2024-09-08 22:56_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4323 on 2024-09-08 22:56_

I checked manually that we do emit a diagnostic informing the user about invalid syntax in this snippet (though the message we give currently isn't great; I wasn't sure at first whether it was a diagnostic or an internal panic!).

It would be nice if `assert_file_diagnostics()` also included diagnostics we emit due to syntax errors so that we got a complete picture of the diagnostics we were emitting. But considering that we're about to implement a whole new set of testing infrastructure, I don't know if that's worth spendingn time on.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:821 on 2024-09-08 22:56_

I _think_ this should be fixed now after my latest pushes!

---

_@AlexWaygood reviewed on 2024-09-08 22:56_

---

_Review requested from @carljm by @AlexWaygood on 2024-09-08 22:56_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:821 on 2024-09-09 02:47_

Yes, looks good!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4240 on 2024-09-09 02:51_

This is more a commentary on testing philosophy than a request for you to change this, but the way I generally think about tests is to aim for each test to be clear about what it's testing, and to be minimalist in every other aspect. This leads to more focused test failures (so when a test fails, you have a good idea where to look for the cause, rather than one breakage causing many seemingly unrelated tests to fail), and tests whose purpose are easier for a reader to understand.

So I would say, we have tests elsewhere (at least I think we do!) that builtins can be accessed either implicitly or via `import builtins`, and nothing in our exception-handler code does or should do anything special regarding this (it's just normal expression type resolution.) So I don't see much reason why the wrinkles of `import builtins` or using an Attribute node as an exception type belong in this test at all. It just makes the reader wonder why they are there, and if they are missing something about their importance.

Of course this philosophy has its limits; some very basic syntactic forms are of course relied on by many tests, because you need them in order to even use the feature you want to test. But both `import builtins` and exception handling are rather specific, less common features, so I would generally not have them in the same test together unless I am intentionally testing some very particular interaction of precisely those two things. And in that case, it would be in a very specific separate test whose name clearly indicates what unusual interaction we are testing, it wouldn't be in a generic `except_handler` test.

---

_@carljm approved on 2024-09-09 03:00_

Looks great!

---

_@AlexWaygood reviewed on 2024-09-09 11:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4240 on 2024-09-09 11:30_

Thanks for the great comment. I did want to include an exception class from another module _somewhere_ in my tests -- it probably isn't _necessary_ since we obviously demonstrate support for cross-module resolution elsewhere, but it felt useful to show that we'd infer the types even across module boundaries. But you're absolutely right that the separation of my tests wasn't very clean! And also, the distinction between `builtins.AttributeError` and `TypeError` was a bad way of demonstrating this, since they are both looked up in the same namespace anyway. I've cleaned the tests up somewhat in https://github.com/astral-sh/ruff/pull/13267/commits/73eb467237a66aa2f79f314015405bb37869fa76

---

_@AlexWaygood reviewed on 2024-09-09 11:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4246 on 2024-09-09 11:32_

I'm not entirely happy that the display of `Type::Instance(re.error)` here is `error` (the fully qualified name would be much more helpful!), but that's definitely out of scope for this PR

---

_Merged by @AlexWaygood on 2024-09-09 11:35_

---

_Closed by @AlexWaygood on 2024-09-09 11:35_

---

_Branch deleted on 2024-09-09 11:35_

---
