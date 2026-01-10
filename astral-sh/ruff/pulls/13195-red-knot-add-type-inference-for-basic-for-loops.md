```yaml
number: 13195
title: "[red-knot] Add type inference for basic `for` loops"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/for-loop-types
created_at: 2024-09-01T15:39:12Z
updated_at: 2024-09-04T10:29:19Z
url: https://github.com/astral-sh/ruff/pull/13195
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] Add type inference for basic `for` loops

---

_Pull request opened by @AlexWaygood on 2024-09-01 15:39_

## Summary

This PR adds type inference for basic `for` loop variables to red-knot.

Inferring the type of a loop var involves either the new-style iteration protocol (which invokes `__iter__` to get an iterator, and then `__next__` on that iterator), or the old-style iteration protocol (which passes incrementally larger `int`s to a `__getitem__` method). If `__iter__` is defined on the class, `__iter__` always takes precedence, even if `__iter__` is e.g. set to `None`: this is a technique that can be used to make classes that define `__getitem__` not-iterable.

Whichever protocol is used to make a class's instances iterable, the dunder methods are always looked up on the _class_ of an instance rather than the instance itself. I.e., the interpreter won't consider `foo` in this snippet to be iterable, because `__iter__` only exists on the instance, not the class:

```pycon
>>> class Foo: ...
... 
>>> foo = Foo()
>>> foo.__iter__ = lambda: iter(range(42))
>>> list(foo.__iter__())
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41]
>>> for x in foo:
...     pass
... 
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'Foo' object is not iterable
```

To emulate this runtime behaviour, I added a `Type::to_type_of_class` method that allows us to go from a type representing `<instance of int>` to a type representing `<the int class itself>`.

## Test Plan

`cargo test`


---

_Label `red-knot` added by @AlexWaygood on 2024-09-01 15:39_

---

_Review requested from @carljm by @AlexWaygood on 2024-09-01 15:39_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-01 15:39_

---

_Comment by @github-actions[bot] on 2024-09-01 15:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-09-01 18:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:385 on 2024-09-01 18:18_

These would be a bit more ergonomic if `Type::member` accepted `&str` rather than `ast::name::Name`. Any reason not to do something like this?

<details>
<summary>Proposed diff</summary>

```diff
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -290,7 +290,7 @@ impl<'db> Type<'db> {
     /// us to explicitly consider whether to handle an error or propagate
     /// it up the call stack.
     #[must_use]
-    pub fn member(&self, db: &'db dyn Db, name: &ast::name::Name) -> Type<'db> {
+    pub fn member(&self, db: &'db dyn Db, name: &str) -> Type<'db> {
         match self {
             Type::Any => Type::Any,
             Type::Never => {
@@ -382,12 +382,10 @@ impl<'db> Type<'db> {
         // `self` represents the type of the iterable;
         // `__iter__` and `__next__` are both looked up on the class of the iterable:
         let type_of_class = self.to_type_of_class(db);
-        let dunder_iter_method = type_of_class.member(db, &ast::name::Name::from("__iter__"));
+        let dunder_iter_method = type_of_class.member(db, "__iter__");
         if !dunder_iter_method.is_unbound() {
             let iterator_ty = dunder_iter_method.call(db)?;
-            let dunder_next_method = iterator_ty
-                .to_type_of_class(db)
-                .member(db, &ast::name::Name::from("__next__"));
+            let dunder_next_method = iterator_ty.to_type_of_class(db).member(db, "__next__");
             return dunder_next_method.call(db);
         }
         // Although it's not considered great practice,
@@ -396,8 +394,7 @@ impl<'db> Type<'db> {
         //
         // TODO this is only valid if the `__getitem__` method is annotated as
         // accepting `int` or `SupportsIndex`
-        let dunder_get_item_method =
-            type_of_class.member(db, &ast::name::Name::from("__getitem__"));
+        let dunder_get_item_method = type_of_class.member(db, "__getitem__");
         dunder_get_item_method.call(db)
     }
 
@@ -516,7 +513,7 @@ impl<'db> ClassType<'db> {
     /// Returns the class member of this class named `name`.
     ///
     /// The member resolves to a member of the class itself or any of its bases.
-    pub fn class_member(self, db: &'db dyn Db, name: &ast::name::Name) -> Type<'db> {
+    pub fn class_member(self, db: &'db dyn Db, name: &str) -> Type<'db> {
         let member = self.own_class_member(db, name);
         if !member.is_unbound() {
             return member;
@@ -526,12 +523,12 @@ impl<'db> ClassType<'db> {
     }
 
     /// Returns the inferred type of the class member named `name`.
-    pub fn own_class_member(self, db: &'db dyn Db, name: &ast::name::Name) -> Type<'db> {
+    pub fn own_class_member(self, db: &'db dyn Db, name: &str) -> Type<'db> {
         let scope = self.body_scope(db);
         symbol_ty_by_name(db, scope, name)
     }
 
-    pub fn inherited_class_member(self, db: &'db dyn Db, name: &ast::name::Name) -> Type<'db> {
+    pub fn inherited_class_member(self, db: &'db dyn Db, name: &str) -> Type<'db> {
         for base in self.bases(db) {
             let member = base.member(db, name);
             if !member.is_unbound() {
```

</details>

---

_@carljm reviewed on 2024-09-01 19:40_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:385 on 2024-09-01 19:40_

Nope no objection to that, I think `&str` makes more sense. 

---

_@AlexWaygood reviewed on 2024-09-01 19:46_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:385 on 2024-09-01 19:46_

Pushed!

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/core_stdlib_modules.rs`:1 on 2024-09-02 11:00_

Nit: I would just name this `stdlib`. Considering that it doesn't expose any `modules`, just methods to work with `stdlib`.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/core_stdlib_modules.rs`:32 on 2024-09-02 11:01_

It's probably unrelated to this PR but I still wonder if it is worth having this as a salsa tracked considering that both `resolve_module` and `global_scope` are salsa ingredients. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:80 on 2024-09-02 11:03_

I would remove this comment. It's unclear to a reader what they should do about it.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:82 on 2024-09-02 11:03_

I find it surprising that this enum isn't part of `stdlib`, especially considering that they have the same variants. 

Nit: I would rename this to `Stdlib` 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:97 on 2024-09-02 11:05_

I'm inclined to inreverse the `CoreStdlibModule` -> `*_scope` dependency:

* add a `module_name` method 
```suggestion
    let types_file = resolve_module(db, self.module_name())?.file();
    Some(global_scope(db, types_file))
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:114 on 2024-09-02 11:06_

Should this also be moved to `stdlib`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:424 on 2024-09-02 11:08_

Nit. Maybe add some blank lines to make it clearer that `__getitem__` is a separate lookup logic. Or use `if...else`
```suggestion
        if dunder_iter_method.is_bound() {
            let iterator_ty = dunder_iter_method.call(db)?;
            let dunder_next_method = iterator_ty.to_type_of_class(db).member(db, "__next__");
            dunder_next_method.call(db);
        } else {
	        // Although it's not considered great practice,
	        // classes that define `__getitem__` are also iterable,
	        // even if they do not define `__iter__`.
	        //
	        // TODO this is only valid if the `__getitem__` method is annotated as
	        // accepting `int` or `SupportsIndex`
	        let dunder_get_item_method = type_of_class.member(db, "__getitem__");
	        dunder_get_item_method.call(db)
        }
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:424 on 2024-09-02 11:09_

Would this logic change once we have proper `Protocol` support?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:468 on 2024-09-02 11:10_

Nit: Should we introduce helper functions like `types_symbol_ty_by_name` and `typeshed_symbol_ty_by_name` similar to `builtins_ty_by_name`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:457 on 2024-09-02 11:11_

Nit: `to_class_type`. Although I can see how this is slightly confusing with `Type::class`. But I think that also applies to `to_type_of_class`. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:556 on 2024-09-02 11:11_

Maybe `map`?


---

_@MichaReiser reviewed on 2024-09-02 11:13_

---

_@AlexWaygood reviewed on 2024-09-02 11:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:82 on 2024-09-02 11:58_

> Nit: I would rename this to `Stdlib`

Well, it's not the intention that this would be an exhuastive enumeration of all stdlib modules, though -- just ones that contain core definitions for fundamental or builtin types, that we'll likely need to examine frequently during semantic analysis

---

_@MichaReiser reviewed on 2024-09-02 12:01_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:82 on 2024-09-02 12:01_

Yeah I assumed so. But it's still all the methods we need from stdlib ;) It's just that for most modules we won't have any special handling.

---

_@AlexWaygood reviewed on 2024-09-02 12:04_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:424 on 2024-09-02 12:04_

> Would this logic change once we have proper `Protocol` support?

I don't think so. It will become more useful when we have proper `Protocol` support, because most `__iter__` methods in Python are annotated as returning `Iterator[...]`, and `Iterator` is a protocol that desugars to "an instance of some class that has `__iter__` and `__next__` methods". So currently this logic does not recognise most Python iterators as actually being Python iterators, but it will do once we add `Protocol` support.

Once we have `Protocol` support, it's true that we _could_ "simplify" this logic by simply saying "anything that's a subtype of `Iterable[object]` counts as an iterable", and use that to determine whether something is iterable or not. I prefer my current logic, though, because:
- the `Iterator` protocol states that an iterator has to have `__next__` and `__iter__`, but this isn't actually required by the language semantics. It's considered good coding style for all iterators to also be iterable (have an `__iter__` method) but the language only requires them to have a `__next__` method.
- Only considering subtypes of `Iterable[object]` to be iterable ignores the fact that classes that use the old-style iteration protocol (using `__getitem__`) are also considered iterable by the language

---

_@AlexWaygood reviewed on 2024-09-02 12:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:424 on 2024-09-02 12:08_

> Or use `if...else`

I tried that first, but Clippy insisted that I put the `__getitem__` logic before the `__iter__` logic:

```rs
if dunder_iter_method.is_unbound() {
    <__getitem__ logic>
} else {
    <__iter__ logic>
}
```

That seems very wrong to me, because the `__getitem__` logic is a fallback if the object doesn't have an `__iter__` method; the `__iter__` logic should definitely come first. We don't have an `is_bound()` method (only an `is_unbound()` method), and I'm not sure we should add one because we don't have a `Type::Bound` variant, only a `Type::Unbound` variant.

I'll add some more blank lines to make it clearer.

---

_@AlexWaygood reviewed on 2024-09-02 12:09_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:457 on 2024-09-02 12:09_

It's very hard to find a good name here... I initially had `to_type()`, but then I realised that would end up as `Type::to_type`, which is awful 😆

---

_@AlexWaygood reviewed on 2024-09-02 13:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/core_stdlib_modules.rs`:32 on 2024-09-02 13:08_

It's possible to get rid of this and the other two as standalone functions, and just have the `Stdlib` enum:

```rs
/// Enumeration of various core stdlib modules
/// which we need frequent access to during semantic analysis
#[derive(Debug, Clone, Copy, PartialEq, Eq)]
pub(crate) enum Stdlib {
    Builtins,
    Types,
    Typeshed,
}

impl Stdlib {
    pub(crate) fn module_name(self) -> &'static str {
        match self {
            Self::Builtins => "builtins",
            Self::Types => "types",
            Self::Typeshed => "_typeshed",
        }
    }

    /// Retrieve the global scope of the given module.
    ///
    /// Returns `None` if the given module isn't available for some reason.
    pub(crate) fn global_scope(self, db: &dyn Db) -> Option<ScopeId<'_>> {
        let module_name = self.module_name();
        let module_name = ModuleName::new_static(module_name)
            .unwrap_or_else(|| panic!("Expected '{module_name}' to be a valid module name"));
        let builtins_file = resolve_module(db, module_name)?.file();
        Some(global_scope(db, builtins_file))
    }

    /// Look up a symbol in the global scope of the given module.
    ///
    /// Returns `Unbound` if the given module isn't available for some reason.
    pub(crate) fn global_symbol<'db>(self, db: &'db dyn Db, name: &str) -> Type<'db> {
        self.global_scope(db)
            .map(|globals| symbol_ty_by_name(db, globals, name))
            .unwrap_or(Type::Unbound)
    }
}
```

All uses of `builtins_symbol_ty_by_name()` would be replaced with `Stdlib::Builtins.global_symbol()`.

I like that design more in lots of ways, but it's also a much bigger diff, and it means that the function to get the builtins scope can't be Salsa-tracked. Overall I'm definitely open to it but I'd rather do it as a followup, as I'm uncertain what impact it would have on performance.

---

_@AlexWaygood reviewed on 2024-09-02 13:14_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:457 on 2024-09-02 13:14_

I prefer my existing name here, I think it's marginally less confusing. But neither is great and I don't have a very strong opinion.

---

_@AlexWaygood reviewed on 2024-09-02 13:15_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:468 on 2024-09-02 13:15_

Could do. It depends on whether we want to keep `builtins_symbol_ty_by_name()` at all (https://github.com/astral-sh/ruff/pull/13195/files#r1740903151)

---

_@AlexWaygood reviewed on 2024-09-02 13:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:97 on 2024-09-02 13:23_

I think this would mean that getting the scope of `builtins`, `types`, etc. could not be Salsa-tracked. I'm open to trying that out but would rather do it in a separate PR so we can look at how it affects performance.

---

_Comment by @MichaReiser on 2024-09-02 13:28_

All changes look good to me but I prefer for @carljm to take a look

---

_@carljm reviewed on 2024-09-03 20:42_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/core_stdlib_modules.rs`:32 on 2024-09-03 20:42_

I think it's probably not worth having these as tracked functions, but fine if you'd rather we do it in a separate PR.

Regarding naming, a few thoughts:

1) I don't think we should have a method `global_symbol` that returns a `Type`, not a `Symbol`; it should be `global_symbol_ty` instead.
 
2) I'm happy to get rid of the `_by_name` suffix and instead rename e.g. `symbol_ty` to `symbol_ty_by_id` -- by name is the common case and can get the shorter name.

3) I don't have strong feelings either way about `Stdlib::Builtins.global_symbol_ty` vs `builtins_symbol_ty`, but I think I mildly prefer the latter. It's less "clean" in a namespacing sense, but we aren't going to hardcode queries to very many stdlib modules, and `builtins_symbol_ty` is both easier to type and easier to read than `Stdlib::Builtins.global_symbol_ty`, with all its punctuation.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:457 on 2024-09-03 20:46_

I prefer `to_class_type`; I'm unable to detect any clarity improvement in `to_type_of_class`. An alternative would be `to_meta_type`, but I don't think that's better than `to_class_type`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/core_stdlib_modules.rs`:32 on 2024-09-03 20:48_

(I also prefer `Stdlib` over `CoreStdlibModule`, but that preference is weaker if we provide helper functions like `builtins_symbol_ty` and don't have to use this enum name everywhere.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:585 on 2024-09-03 20:49_

nice!!

---

_@carljm approved on 2024-09-03 20:51_

This is great!! So neat to see things coming together such that we can now do real inference based on dunder protocols :)

---

_@AlexWaygood reviewed on 2024-09-03 20:55_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:457 on 2024-09-03 20:55_

Oh I actually like `to_meta_type` quite a lot! I think it's much clearer that you might not end up with a `Type::Class` variant or `ClassType` instance at the end of it. Mind if I go with that?

---

_@carljm reviewed on 2024-09-03 21:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:457 on 2024-09-03 21:04_

No, I have no problem going with that!

---

_@dhruvmanila reviewed on 2024-09-04 04:39_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/stdlib.rs`:52 on 2024-09-04 04:39_

small nit: as per https://doc.rust-lang.org/stable/std/error/index.html#common-message-styles, I guess this should be `'builtins' should be a valid module name` and similar suggestion for other `expect` messages

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/stdlib.rs`:52 on 2024-09-04 09:12_

Oh, thanks for linking to that, I hadn't read that page before! These docs might be interesting for @carljm as well

---

_@AlexWaygood reviewed on 2024-09-04 09:12_

---

_@AlexWaygood reviewed on 2024-09-04 09:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/core_stdlib_modules.rs`:32 on 2024-09-04 09:43_

Well, it will definitely be a simplification, so if you both think it's probably not worth it for these to be Salsa-tracked, I'll defer to you! 🤞 that it doesn't lead to Codspeed complaining 😆

---

_Merged by @AlexWaygood on 2024-09-04 10:19_

---

_Closed by @AlexWaygood on 2024-09-04 10:19_

---

_Branch deleted on 2024-09-04 10:19_

---
