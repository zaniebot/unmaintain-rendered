```yaml
number: 13384
title: "[red-knot] support for typing.reveal_type"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/reveal-type
created_at: 2024-09-17T18:41:57Z
updated_at: 2024-09-19T18:45:14Z
url: https://github.com/astral-sh/ruff/pull/13384
synced_at: 2026-01-10T21:30:32Z
```

# [red-knot] support for typing.reveal_type

---

_Pull request opened by @carljm on 2024-09-17 18:41_

Add support for the `typing.reveal_type` function, emitting a diagnostic revealing the type of its single argument. This is a necessary piece for the planned testing framework.

This puts the cart slightly in front of the horse, in that we don't yet have proper support for validating call signatures / argument types. But it's easy to do just enough to make `reveal_type` work.

This PR includes support for calling union types (this is necessary because we don't yet support `sys.version_info` checks, so `typing.reveal_type` itself is a union type), plus some nice consolidated error messages for calls to unions where some elements are not callable. This is mostly to demonstrate the flexibility in diagnostics that we get from the `CallOutcome` enum.

---

_Label `red-knot` added by @carljm on 2024-09-17 18:41_

---

_Renamed from "[red-knot] [WIP] typing.reveal_type" to "[red-knot] support for typing.reveal_type" by @carljm on 2024-09-17 21:34_

---

_Comment by @github-actions[bot] on 2024-09-17 21:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @carljm on 2024-09-17 22:07_

---

_Review requested from @MichaReiser by @carljm on 2024-09-17 22:07_

---

_Review requested from @AlexWaygood by @carljm on 2024-09-17 22:07_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:242 on 2024-09-18 06:28_

Do we need to store the entire `FunctionType` for `RevealType`? Don't we know its exact signature? 

Also, kind of funny to have this as a dedicated type

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:334 on 2024-09-18 06:29_

I guess the answer is probably *no* because the function is called `into_function_type` and not `into_function` but is there any existing code that makes the assumption that the function only returns `Some` if the variant is `Function`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:504 on 2024-09-18 06:31_

Should this be `None` instead for better error messages? It's a difference whether a type is inferred as `Unknown` or the argument was missing.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:525 on 2024-09-18 06:33_

Nit: I would expect a `CallOutcome::union` constructor function for symmetry with `::callable`. Or to move `Type::call to `CallOutcome::new(Type)`.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:524 on 2024-09-18 06:35_

Do we need to unify the elements because multiple elements could have the same return type (or even all of them)?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:503 on 2024-09-18 06:36_

Is this a fixed return type or is the `reveal_type` function similar to `dbg` where it is an `identity` function with the side effect that it prints the type?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:755 on 2024-09-18 06:39_

Nit: I would move out the `if` because it also has a side effect (and the formatting is rather weird). 
```suggestion
												let call_ty = if let Self::NotCallable { not_callable_ty } = outcome {
													not_callable.push(*not_callable_ty);
													Type::Unknown
												} else {
													outcome.unwrap_with_diagnostic(db, node, builder)
												}
                        union_builder.add(call_ty);
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2418 on 2024-09-18 06:42_

What's the motivation for this change? I can't find any use of `add_diagnostic_string` outside of `add_diagnostic?

I would prefer not to expose a `_string` function to avoid usage of `format!`. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:656 on 2024-09-18 06:43_

I find the name `Outcome` a bit strange but I think that's something you plan to address as part of your context proposal. 

---

_@MichaReiser approved on 2024-09-18 06:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:242 on 2024-09-18 12:30_

Yeah, agree with Micha that there's not much use storing inner data for this variant, since we already have the complexity of having an extra variant (wrapping inner data doesn't really save us any complexity, I don't think), everything about `reveal_type`'s special behaviour will have to be hardcoded in by us anyway, and the signature is known and guaranteed to never change.

> Also, kind of funny to have this as a dedicated type

It surprised me initially that this was the approach used, since conceptually it's really the _symbol_ of `reveal_type` that's special rather than the _type_ of `reveal_type`. But I suppose that unless it's represented as a separate variant here, it's hard for `Type::Call` to operate differently when applied to `revealed_type` as opposed to other functions.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:334 on 2024-09-18 12:30_

I think this makes `Type::expect_function` not operate as expected, since the purpose of that method is partly to assert that the variant you have is a `Type::Function` variant:

https://github.com/astral-sh/ruff/blob/4eb849aed3a6df9ce0f45dd7a2fdb525e31e61e3/crates/red_knot_python_semantic/src/types.rs#L332-L335

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:378 on 2024-09-18 12:31_

Is this correct? It surprises me that `Type::RevealedType(...).is_stdlib_symbol(&db, "collections", "deque")` would return `true`. It's not the stdlib symbol `collections.deque`!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:503 on 2024-09-18 12:32_

It's a generic identity function, the same as `dbg!`: https://github.com/python/cpython/blob/32119fc377a4d9df524a7bac02b6922a990361dd/Lib/typing.py#L3635-L3651

---

_@AlexWaygood reviewed on 2024-09-18 12:32_

---

_@carljm reviewed on 2024-09-18 15:45_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:242 on 2024-09-18 15:45_

## Why have this as a distinct Type variant?

Conceptually, singleton types (which include Class and Function) are already the same thing as "a symbol" -- they represent a single Python class or function object, defined at a single location, and these types allow us to track this identity across imports (including with aliasing), assignments, etc. So identifying `reveal_type` via the type system is a natural fit; I don't really see what the alternative would even be.

In principle, we wouldn't _need_ the extra variant for this; `Type::Function` has all the information we need already. But practically speaking, it would be a bad idea for performance if we had to add an extra check "wait, is this function `reveal_type`??" at every single call site of any function; `is_stdlib_symbol` is not free, differentiating an enum tag is much much cheaper. Using a distinct variant allows us to make the `is_stdlib_symbol` check only at function-definition time, which is _much_ less frequent than function-call time.

## Should we store the inner `FunctionType` inside `Type::RevealTypeFunction`?

I don't have super-strong feelings here, but I think the overall cost-benefit for doing this is positive.

The cost is very low; a `FunctionType` is just a u32 id, and `RevealTypeFunction` is not going to be a super common type. I would venture to say the cost of doing this is effectively zero.

And the benefit is not zero. We don't _have_ to hardcode _everything_ about `RevealTypeFunction`. If we store the inner `FunctionType`, we can in almost every case just fall back to the normal `Function` handling, and only add special `reveal_type` behavior where it is actually special. For instance, I would rather not hardcode call-signature validation, I'd rather share as much as possible with `Type::Function`.

So yes, we could skip storing the inner `FunctionType`, and instead hardcode literally everything about `RevealTypeFunction`. But I don't see why we'd want to do this, when we don't have to.

---

_@carljm reviewed on 2024-09-18 15:51_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:334 on 2024-09-18 15:51_

> is there any existing code that makes the assumption that the function only returns `Some` if the variant is `Function`?

No, there's no such code. And I think we generally want any external code to ideally transparently treat `Type::RevealTypeFunction` as if it were `Type::Function`. Because `reveal_type` is a function, and should behave as one.

This is another really good reason to store the inner `FunctionType` inside `RevealTypeFunction`.

> I think this makes `Type::expect_function` not operate as expected

I think this makes `Type::expect_function` behave precisely how we want it to, which is that the caller can transparently get out the correct `FunctionType` without having to special-case `RevealTypeFunction`. Once the caller has the `FunctionType`, there's no reason they should have to know or care about whether it was a `Type::Function` or `Type::RevealTypeFunction`.

---

_@AlexWaygood reviewed on 2024-09-18 15:55_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:334 on 2024-09-18 15:55_

> I think this makes `Type::expect_function` behave precisely how we want it to

Well, you'll at least need to change the message in the `.expect()` call here, to something like "expected a variant that wraps a `FunctionType`"

https://github.com/astral-sh/ruff/blob/4eb849aed3a6df9ce0f45dd7a2fdb525e31e61e3/crates/red_knot_python_semantic/src/types.rs#L332-L335

---

_@carljm reviewed on 2024-09-18 15:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:504 on 2024-09-18 15:57_

Yes, but this shouldn't be handled as a special case for `CallOutcome::RevealType`, it should be handled by general call-argument-matching-and-validation, which applies to all function calls (and which we don't have yet.) So this code is presuming that in the future (once we add call signature checking) you would get a diagnostic from that; we don't need extra handling it for it here, we should just reveal unknown.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:524 on 2024-09-18 15:58_

That's handled by `CallOutcome::return_ty` and `CallOutcome::unwrap_with_diagnostic`, in the `CallOutcome::Union` arms, where we do build a union of all the return types.

---

_@carljm reviewed on 2024-09-18 15:58_

---

_@carljm reviewed on 2024-09-18 15:59_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:503 on 2024-09-18 15:59_

It would be possible here to instead set `return_ty` to the same as the revealed type. But this is another case where I would prefer to have less special-casing, and allow the return type to be inferred via the typeshed stubs, the same as for any other function. The result should be the same, although avoiding special-casing means we won't have that correct result until we add support for generic functions.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2418 on 2024-09-18 16:01_

Oops, that was an accidental holdover from a different aborted approach. Removed!

---

_@carljm reviewed on 2024-09-18 16:01_

---

_@carljm reviewed on 2024-09-18 16:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:378 on 2024-09-18 16:04_

Oops, no, this is wrong, good catch! I originally had it correct, and then in a review pass I was like "wait, we always know it's a stdlib symbol", forgetting that the semantics of the function are not "is it a stdlib symbol" but rather "is it _this_ stdlib symbol". Fixed.

---

_@MichaReiser reviewed on 2024-09-18 16:15_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:504 on 2024-09-18 16:15_

Possibly. But then you end up with two diagnostics where one is the error and the other is the reveal type diagnostic which doesn't seem ideal to me. But I can see that special casing reveal type's diagnostic is probably more work.

---

_@carljm reviewed on 2024-09-18 16:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:334 on 2024-09-18 16:22_

Good call! Fixed the message.

---

_@carljm reviewed on 2024-09-18 16:24_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:525 on 2024-09-18 16:24_

Maybe I should just remove these constructors, I'm not sure they are worth it. But for now I added new ones for `revealed` and `union`, for symmetry.

Eliminating `Type::call` is an interesting idea, but to me it seems clearer to keep it.

---

_@carljm reviewed on 2024-09-18 16:27_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:656 on 2024-09-18 16:27_

I don't have any plans to address this. My document was just listing out pros and cons as I saw them of different options I'd considered in working on this, so as to be able to refer back to them later. My conclusion for now was to _not_ do a context object, and stick with this style.

It's very possible that we later decide it makes sense to do a type inference context object, but that's not clear to me yet and not something I plan to do in the short term.

So if you don't like the name "Outcome" then we should brainstorm alternative name options, and we can change both `CallOutcome` and `IterationOutcome`. But maybe in a separate PR.

---

_@carljm reviewed on 2024-09-18 16:40_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:504 on 2024-09-18 16:40_

Good point that it may not make sense to even have a revealed-type diagnostic if the call to `reveal_type` is incorrect. But when we add call-signature checking (which can be shared in a common function used across both the `Function` and `RevealTypeFunction` arms, because it just needs the call arguments and the `FunctionType`), the `RevealTypeFunction` arm isn't required to return a `CallOutcome::RevealType` at all. If it fails call signature checking, it can return a different outcome type, that won't issue a revealed-type diagnostic at all.

So in that case you can consider this fallback just a temporary band-aid until we have call signature checking (which I already added a TODO for above.)

---

_Merged by @carljm on 2024-09-18 16:59_

---

_Closed by @carljm on 2024-09-18 16:59_

---

_Branch deleted on 2024-09-18 16:59_

---

_@AlexWaygood reviewed on 2024-09-19 01:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:242 on 2024-09-19 01:31_

> I don't really see what the alternative would even be.

The alternative would be this diff, right? All the red-knot tests pass if I apply this diff to `main`, and to me it seems much simpler conceptually. Perhaps it adds a performance cost, though, because you have to check whether the function is the `reveal_type` function every time you run `Type::Call`.

<details>
<summary>Diff demonstrating an implementation without a new `Type` variant</summary>

```diff
diff --git a/crates/red_knot_python_semantic/src/types.rs b/crates/red_knot_python_semantic/src/types.rs
index 607fa80ac..38451fd9c 100644
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -238,8 +238,6 @@ pub enum Type<'db> {
     None,
     /// a specific function object
     Function(FunctionType<'db>),
-    /// The `typing.reveal_type` function, which has special `__call__` behavior.
-    RevealTypeFunction(FunctionType<'db>),
     /// a specific module object
     Module(File),
     /// a specific class object
@@ -326,9 +324,7 @@ impl<'db> Type<'db> {
 
     pub const fn into_function_type(self) -> Option<FunctionType<'db>> {
         match self {
-            Type::Function(function_type) | Type::RevealTypeFunction(function_type) => {
-                Some(function_type)
-            }
+            Type::Function(function_type) => Some(function_type),
             _ => None,
         }
     }
@@ -374,9 +370,7 @@ impl<'db> Type<'db> {
     pub fn is_stdlib_symbol(&self, db: &'db dyn Db, module_name: &str, name: &str) -> bool {
         match self {
             Type::Class(class) => class.is_stdlib_symbol(db, module_name, name),
-            Type::Function(function) | Type::RevealTypeFunction(function) => {
-                function.is_stdlib_symbol(db, module_name, name)
-            }
+            Type::Function(function) => function.is_stdlib_symbol(db, module_name, name),
             _ => false,
         }
     }
@@ -450,7 +444,7 @@ impl<'db> Type<'db> {
                 // TODO: attribute lookup on None type
                 Type::Unknown
             }
-            Type::Function(_) | Type::RevealTypeFunction(_) => {
+            Type::Function(_) => {
                 // TODO: attribute lookup on function type
                 Type::Unknown
             }
@@ -499,11 +493,16 @@ impl<'db> Type<'db> {
     fn call(self, db: &'db dyn Db, arg_types: &[Type<'db>]) -> CallOutcome<'db> {
         match self {
             // TODO validate typed call arguments vs callable signature
-            Type::Function(function_type) => CallOutcome::callable(function_type.return_type(db)),
-            Type::RevealTypeFunction(function_type) => CallOutcome::revealed(
-                function_type.return_type(db),
-                *arg_types.first().unwrap_or(&Type::Unknown),
-            ),
+            Type::Function(function_type) => {
+                if function_type.is_stdlib_symbol(db, "typing", "reveal_type") {
+                    CallOutcome::revealed(
+                        function_type.return_type(db),
+                        *arg_types.first().unwrap_or(&Type::Unknown),
+                    )
+                } else {
+                    CallOutcome::callable(function_type.return_type(db))
+                }
+            }
 
             // TODO annotated return type on `__new__` or metaclass `__call__`
             Type::Class(class) => CallOutcome::callable(Type::Instance(class)),
@@ -605,7 +604,6 @@ impl<'db> Type<'db> {
             Type::BooleanLiteral(_)
             | Type::BytesLiteral(_)
             | Type::Function(_)
-            | Type::RevealTypeFunction(_)
             | Type::Instance(_)
             | Type::Module(_)
             | Type::IntLiteral(_)
@@ -628,7 +626,7 @@ impl<'db> Type<'db> {
             Type::BooleanLiteral(_) => builtins_symbol_ty(db, "bool"),
             Type::BytesLiteral(_) => builtins_symbol_ty(db, "bytes"),
             Type::IntLiteral(_) => builtins_symbol_ty(db, "int"),
-            Type::Function(_) | Type::RevealTypeFunction(_) => types_symbol_ty(db, "FunctionType"),
+            Type::Function(_) => types_symbol_ty(db, "FunctionType"),
             Type::Module(_) => types_symbol_ty(db, "ModuleType"),
             Type::None => typeshed_symbol_ty(db, "NoneType"),
             // TODO not accurate if there's a custom metaclass...
diff --git a/crates/red_knot_python_semantic/src/types/display.rs b/crates/red_knot_python_semantic/src/types/display.rs
index 3d037fe65..954a19d31 100644
--- a/crates/red_knot_python_semantic/src/types/display.rs
+++ b/crates/red_knot_python_semantic/src/types/display.rs
@@ -36,7 +36,6 @@ impl Display for DisplayType<'_> {
                 | Type::BytesLiteral(_)
                 | Type::Class(_)
                 | Type::Function(_)
-                | Type::RevealTypeFunction(_)
         ) {
             write!(f, "Literal[{representation}]",)
         } else {
@@ -73,9 +72,7 @@ impl Display for DisplayRepresentation<'_> {
             // TODO functions and classes should display using a fully qualified name
             Type::Class(class) => f.write_str(class.name(self.db)),
             Type::Instance(class) => f.write_str(class.name(self.db)),
-            Type::Function(function) | Type::RevealTypeFunction(function) => {
-                f.write_str(function.name(self.db))
-            }
+            Type::Function(function) => f.write_str(function.name(self.db)),
             Type::Union(union) => union.display(self.db).fmt(f),
             Type::Intersection(intersection) => intersection.display(self.db).fmt(f),
             Type::IntLiteral(n) => n.fmt(f),
@@ -194,7 +191,7 @@ impl TryFrom<Type<'_>> for LiteralTypeKind {
     fn try_from(value: Type<'_>) -> Result<Self, Self::Error> {
         match value {
             Type::Class(_) => Ok(Self::Class),
-            Type::Function(_) | Type::RevealTypeFunction(_) => Ok(Self::Function),
+            Type::Function(_) => Ok(Self::Function),
             Type::IntLiteral(_) => Ok(Self::IntLiteral),
             Type::StringLiteral(_) => Ok(Self::StringLiteral),
             Type::BytesLiteral(_) => Ok(Self::BytesLiteral),
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index 52d95f1be..c53ce7b27 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -704,12 +704,12 @@ impl<'db> TypeInferenceBuilder<'db> {
             }
         }
 
-        let function_type = FunctionType::new(self.db, name.id.clone(), definition, decorator_tys);
-        let function_ty = if function_type.is_stdlib_symbol(self.db, "typing", "reveal_type") {
-            Type::RevealTypeFunction(function_type)
-        } else {
-            Type::Function(function_type)
-        };
+        let function_ty = Type::Function(FunctionType::new(
+            self.db,
+            name.id.clone(),
+            definition,
+            decorator_tys,
+        ));
 
         self.add_declaration_with_binding(function.into(), definition, function_ty, function_ty);
     }
```

---

_@carljm reviewed on 2024-09-19 01:35_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:242 on 2024-09-19 01:35_

Sure, yeah -- that's what I addressed in my second paragraph. I guess my first paragraph didn't make much sense / wasn't relevant, because you're not suggesting not using types at all.

This approach is definitely simpler, but I considered it a performance nonstarter to add the overhead of `is_stdlib_symbol` to every Type::call. Perhaps that's premature optimization. 

---

_@AlexWaygood reviewed on 2024-09-19 01:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:242 on 2024-09-19 01:39_

I expect `Type::Call` to be pretty darn hot (even attribute accesses may need to invoke it once we implement the descriptor protocol), so I think it's reasonable to worry about the performance of the method. Just as long as we're on the same page about _why_ we're using a separate variant ;)

---

_@AlexWaygood reviewed on 2024-09-19 14:45_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:242 on 2024-09-19 14:45_

Yet another option might be to add a boolean field (which in the long term would probably be better as a bitflag or enum field) to `FunctionType`. We could determine once when we create `FunctionType` objects whether it's a regular function or a "special" function (`reveal_type`/`assert_type`/`cast`/`final`/`overload`/`override`/`no_type_check`/`type_check_only`/`runtime_checkable`/`dataclasses.dataclass`/`dataclasses.field`/`enum.unique`/`enum.member`/`enum.nonmember`/`dataclass_transform`/`warnings.deprecated`/etc.), and store that information on `FunctionType` in such a way that it could be cheaply queried from `Type::Call`:

<details>

```diff
diff --git a/crates/red_knot_python_semantic/src/semantic_index/definition.rs b/crates/red_knot_python_semantic/src/semantic_index/definition.rs
index 35b1fefc9..0104515af 100644
--- a/crates/red_knot_python_semantic/src/semantic_index/definition.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/definition.rs
@@ -3,6 +3,7 @@ use ruff_db::parsed::ParsedModule;
 use ruff_python_ast as ast;
 
 use crate::ast_node_ref::AstNodeRef;
+use crate::module_resolver::file_to_module;
 use crate::node_key::NodeKey;
 use crate::semantic_index::symbol::{FileScopeId, ScopeId, ScopedSymbolId};
 use crate::Db;
@@ -45,6 +46,14 @@ impl<'db> Definition<'db> {
     pub(crate) fn is_binding(self, db: &'db dyn Db) -> bool {
         self.kind(db).category().is_binding()
     }
+
+    /// Return true if this is a symbol was defined in the `typing` or `typing_extensions` modules
+    pub(crate) fn is_typing_definition(self, db: &'db dyn Db) -> bool {
+        file_to_module(db, self.file(db)).is_some_and(|module| {
+            module.search_path().is_standard_library()
+                && matches!(&**module.name(), "typing" | "typing_extensions")
+        })
+    }
 }
 
 #[derive(Copy, Clone, Debug)]
diff --git a/crates/red_knot_python_semantic/src/types.rs b/crates/red_knot_python_semantic/src/types.rs
index 31355eb48..f31b13f81 100644
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -245,8 +245,6 @@ pub enum Type<'db> {
     None,
     /// a specific function object
     Function(FunctionType<'db>),
-    /// The `typing.reveal_type` function, which has special `__call__` behavior.
-    RevealTypeFunction(FunctionType<'db>),
     /// a specific module object
     Module(File),
     /// a specific class object
@@ -333,9 +331,7 @@ impl<'db> Type<'db> {
 
     pub const fn into_function_type(self) -> Option<FunctionType<'db>> {
         match self {
-            Type::Function(function_type) | Type::RevealTypeFunction(function_type) => {
-                Some(function_type)
-            }
+            Type::Function(function_type) => Some(function_type),
             _ => None,
         }
     }
@@ -381,9 +377,7 @@ impl<'db> Type<'db> {
     pub fn is_stdlib_symbol(&self, db: &'db dyn Db, module_name: &str, name: &str) -> bool {
         match self {
             Type::Class(class) => class.is_stdlib_symbol(db, module_name, name),
-            Type::Function(function) | Type::RevealTypeFunction(function) => {
-                function.is_stdlib_symbol(db, module_name, name)
-            }
+            Type::Function(function) => function.is_stdlib_symbol(db, module_name, name),
             _ => false,
         }
     }
@@ -475,7 +469,7 @@ impl<'db> Type<'db> {
                 // TODO: attribute lookup on None type
                 Type::Unknown
             }
-            Type::Function(_) | Type::RevealTypeFunction(_) => {
+            Type::Function(_) => {
                 // TODO: attribute lookup on function type
                 Type::Unknown
             }
@@ -524,11 +518,16 @@ impl<'db> Type<'db> {
     fn call(self, db: &'db dyn Db, arg_types: &[Type<'db>]) -> CallOutcome<'db> {
         match self {
             // TODO validate typed call arguments vs callable signature
-            Type::Function(function_type) => CallOutcome::callable(function_type.return_type(db)),
-            Type::RevealTypeFunction(function_type) => CallOutcome::revealed(
-                function_type.return_type(db),
-                *arg_types.first().unwrap_or(&Type::Unknown),
-            ),
+            Type::Function(function_type) => {
+                if function_type.is_reveal_type(db) {
+                    CallOutcome::revealed(
+                        function_type.return_type(db),
+                        *arg_types.first().unwrap_or(&Type::Unknown),
+                    )
+                } else {
+                    CallOutcome::callable(function_type.return_type(db))
+                }
+            }
 
             // TODO annotated return type on `__new__` or metaclass `__call__`
             Type::Class(class) => CallOutcome::callable(Type::Instance(class)),
@@ -630,7 +629,6 @@ impl<'db> Type<'db> {
             Type::BooleanLiteral(_)
             | Type::BytesLiteral(_)
             | Type::Function(_)
-            | Type::RevealTypeFunction(_)
             | Type::Instance(_)
             | Type::Module(_)
             | Type::IntLiteral(_)
@@ -653,7 +651,7 @@ impl<'db> Type<'db> {
             Type::BooleanLiteral(_) => builtins_symbol_ty(db, "bool"),
             Type::BytesLiteral(_) => builtins_symbol_ty(db, "bytes"),
             Type::IntLiteral(_) => builtins_symbol_ty(db, "int"),
-            Type::Function(_) | Type::RevealTypeFunction(_) => types_symbol_ty(db, "FunctionType"),
+            Type::Function(_) => types_symbol_ty(db, "FunctionType"),
             Type::Module(_) => types_symbol_ty(db, "ModuleType"),
             Type::None => typeshed_symbol_ty(db, "NoneType"),
             // TODO not accurate if there's a custom metaclass...
@@ -864,6 +862,9 @@ pub struct FunctionType<'db> {
     #[return_ref]
     pub name: ast::name::Name,
 
+    /// Flag to indicate that this function is `typing(_extensions).reveal_type`
+    is_reveal_type: bool,
+
     definition: Definition<'db>,
 
     /// types of all decorators on this function
@@ -879,15 +880,6 @@ impl<'db> FunctionType<'db> {
             })
     }
 
-    /// Return true if this is a symbol with given name from `typing` or `typing_extensions`.
-    pub(crate) fn is_typing_symbol(self, db: &'db dyn Db, name: &str) -> bool {
-        name == self.name(db)
-            && file_to_module(db, self.definition(db).file(db)).is_some_and(|module| {
-                module.search_path().is_standard_library()
-                    && matches!(&**module.name(), "typing" | "typing_extensions")
-            })
-    }
-
     pub fn has_decorator(self, db: &dyn Db, decorator: Type<'_>) -> bool {
         self.decorators(db).contains(&decorator)
     }
diff --git a/crates/red_knot_python_semantic/src/types/display.rs b/crates/red_knot_python_semantic/src/types/display.rs
index 3d037fe65..954a19d31 100644
--- a/crates/red_knot_python_semantic/src/types/display.rs
+++ b/crates/red_knot_python_semantic/src/types/display.rs
@@ -36,7 +36,6 @@ impl Display for DisplayType<'_> {
                 | Type::BytesLiteral(_)
                 | Type::Class(_)
                 | Type::Function(_)
-                | Type::RevealTypeFunction(_)
         ) {
             write!(f, "Literal[{representation}]",)
         } else {
@@ -73,9 +72,7 @@ impl Display for DisplayRepresentation<'_> {
             // TODO functions and classes should display using a fully qualified name
             Type::Class(class) => f.write_str(class.name(self.db)),
             Type::Instance(class) => f.write_str(class.name(self.db)),
-            Type::Function(function) | Type::RevealTypeFunction(function) => {
-                f.write_str(function.name(self.db))
-            }
+            Type::Function(function) => f.write_str(function.name(self.db)),
             Type::Union(union) => union.display(self.db).fmt(f),
             Type::Intersection(intersection) => intersection.display(self.db).fmt(f),
             Type::IntLiteral(n) => n.fmt(f),
@@ -194,7 +191,7 @@ impl TryFrom<Type<'_>> for LiteralTypeKind {
     fn try_from(value: Type<'_>) -> Result<Self, Self::Error> {
         match value {
             Type::Class(_) => Ok(Self::Class),
-            Type::Function(_) | Type::RevealTypeFunction(_) => Ok(Self::Function),
+            Type::Function(_) => Ok(Self::Function),
             Type::IntLiteral(_) => Ok(Self::IntLiteral),
             Type::StringLiteral(_) => Ok(Self::StringLiteral),
             Type::BytesLiteral(_) => Ok(Self::BytesLiteral),
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index fa5197f5f..865aeeef9 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -711,12 +711,13 @@ impl<'db> TypeInferenceBuilder<'db> {
             }
         }
 
-        let function_type = FunctionType::new(self.db, name.id.clone(), definition, decorator_tys);
-        let function_ty = if function_type.is_typing_symbol(self.db, "reveal_type") {
-            Type::RevealTypeFunction(function_type)
-        } else {
-            Type::Function(function_type)
-        };
+        let function_ty = Type::Function(FunctionType::new(
+            self.db,
+            name.id.clone(),
+            name == "reveal_type" && definition.is_typing_definition(self.db),
+            definition,
+            decorator_tys,
+        ));
 
         self.add_declaration_with_binding(function.into(), definition, function_ty, function_ty);
     }
```

</details>

---

_@carljm reviewed on 2024-09-19 16:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:242 on 2024-09-19 16:08_

Yes, this is an option worth considering. I didn't go with it here because it has a higher memory cost (every function type, even the vast majority of non-special ones, now takes up an extra 8 bytes); I was pretty focused on doing this in a way that adds near-zero cost for the common case. But it may be over time that `FunctionType` is large enough that this isn't a significant additional cost, and/or we end up needing a set of bitflags anyway so it's no extra cost. Or we have so many special function types that representing them all as `Type` variants bumps up the size of the `Type` enum, and that ends up being higher cost. So I can envision lots of potential reasons why it might make sense to switch to the flag approach.

---

_@AlexWaygood reviewed on 2024-09-19 16:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:242 on 2024-09-19 16:42_

> Or we have so many special function types that representing them all as `Type` variants bumps up the size of the `Type` enum, and that ends up being higher cost.

@MichaReiser can correct me if I'm wrong, but I don't believe the number of variants in an enum has an impact on the size of an enum -- I think an enum always just has the size of its largest variant. (Possibly it leads to a larger amount of generated code if you have more variants, though, since the compiler has to generate different code paths for each possible branch?) I'm more concerned about the maintenance cost of having so many possible function-like `Type` variants representing "special functions". ISTM that there's an implicit invariant that they should all work exactly the same way as `Type::Function` in nearly every respect (most of them will only need special handling in `Type::Call`, I think?).

---

_@carljm reviewed on 2024-09-19 17:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:242 on 2024-09-19 17:08_

Yes, I agree the code maintenance is a bigger concern as we accumulate lots of these.

If the contained data inside enum variants is large, that will dominate the overall size of the enum. But in this case the contained data is small (usually just a u32 id; I think `IntLiteral` is currently our largest variant, as it contains an `i64`), so the need to find niches to discriminate the enum variants themselves can be relevant. Though in practice in this case there just aren't any niches available in an `i64`, so the enum requires another entire 8 bytes for the discriminant (doubling the overall size of `Type` to 16 bytes), and we are certainly not going to reach 2^64 variants 😆 

---

_@AlexWaygood reviewed on 2024-09-19 18:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:242 on 2024-09-19 18:19_

> doubling the overall size of `Type` to 16 bytes

I believe a `FunctionType` instance is actually just a `u32`, though, since it's a salsa-tracked struct — right? So adding a boolean field to `FunctionType` would increase the size of the underlying data stored in the salsa cache for each `FunctionType` instance, but wouldn't actually increase the size of the `Type` enum...?

---

_@carljm reviewed on 2024-09-19 18:21_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:242 on 2024-09-19 18:21_

Yes, that's right. I never mentioned the size of the Type enum in relation to adding a boolean to FunctionType. I only brought up the size of the Type enum in relation to having a ton of extra Type variants in the future, and it's probably not even really relevant there.

Making every FunctionType bigger is less bad than making the Type enum bigger would be, but we will have quite a large number of FunctionType in the db.

---

_@AlexWaygood reviewed on 2024-09-19 18:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:242 on 2024-09-19 18:43_

Sorry, that was a sloppy misreading of what you said there!

---

_@carljm reviewed on 2024-09-19 18:45_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:242 on 2024-09-19 18:45_

No worries!

---
