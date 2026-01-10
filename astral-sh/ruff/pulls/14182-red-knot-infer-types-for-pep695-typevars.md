```yaml
number: 14182
title: "[red-knot] infer types for PEP695 typevars"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/typevars
created_at: 2024-11-08T00:21:44Z
updated_at: 2025-05-07T15:23:06Z
url: https://github.com/astral-sh/ruff/pull/14182
synced_at: 2026-01-10T18:57:02Z
```

# [red-knot] infer types for PEP695 typevars

---

_Pull request opened by @carljm on 2024-11-08 00:21_

## Summary

Create definitions and infer types for PEP 695 type variables.

This just gives us the type of the type variable itself (the type of `T` as a runtime object in the body of `def f[T](): ...`), with special handling for its attributes `__name__`, `__bound__`, `__constraints__`, and `__default__`. Mostly the support for these attributes exists because it is easy to implement and allows testing that we are internally representing the typevar correctly.

This PR doesn't yet have support for interpreting a typevar as a type annotation, which is of course the primary use of a typevar. But the information we store in the typevar's type in this PR gives us everything we need to handle it correctly in a future PR when the typevar appears in an annotation.

## Test Plan

Added mdtest.

---

_Label `red-knot` added by @carljm on 2024-11-08 00:21_

---

_Review requested from @MichaReiser by @carljm on 2024-11-08 00:21_

---

_Review requested from @AlexWaygood by @carljm on 2024-11-08 00:21_

---

_Review requested from @sharkdp by @carljm on 2024-11-08 00:21_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics.md`:76 on 2024-11-08 00:36_

These types could more precisely be class literals; `Literal[A]` instead of `type[A]`. But we want to store them as the instance type they represent (that is, interpreted as a type expression, not a value expression), because the priority is efficiency of using typevars as type annotations, not introspecting their dunder attributes. So that means this introspection is effectively round-tripping the type through a `.to_instance()` and then back through `.to_meta_type()`. Since a bound can be any kind of type, we have to do this round-trip generically, which means we get back `type[A]` from that round trip, not `Literal[A]`.

With some special-casing of `Type::Instance` -> `Type::ClassLiteral` we _could_ get `Literal[A]` here, but I don't think it's worth it. Precision of these dunder types matters very little, and `type[]` will be more familiar to users anyway.

---

_@carljm reviewed on 2024-11-08 00:36_

---

_Comment by @github-actions[bot] on 2024-11-08 01:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1715 on 2024-11-08 07:45_

```suggestion
                    typevar
                        .constraints(db)
                        .iter()
                        .map(|ty| ty.to_meta_type(db))
                        .collect(),
```

`FromIter` is implemented for `Box<[T]>` so it should be necessary to go to `Vec` and then `into_boxed_slice` (the implementation probably does but `collect` should hide this for you. You may have to type hint `collect::<Box<_>>`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1383 on 2024-11-08 07:48_

Could we store the `TupleType` in the constraints instead of cloning the `elements` vec? 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1401 on 2024-11-08 07:50_

`Name` implements `salsa::interned::Lookup`. You can see how it is used in the `FunctionType::new` calls

---

_@MichaReiser approved on 2024-11-08 07:51_

---

_@sharkdp reviewed on 2024-11-08 09:18_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1778 on 2024-11-08 09:18_

Minor: The `name` field is `pub`, the bound_or_constraints` field is private, and `default_ty` is `pub(crate)`. Is this intentional? All of them could be private (for now).

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1758 on 2024-11-08 09:32_

Given that a list of constraints is interpreted as: *can only ever be solved as being exactly one of the constraints*, it seems a bit strange to me to map the "no constraints" case to an empty list (which could also mean: can't be solved at all, because there are no options given). I realize that both `mypy` and `pyright` disallow this (*"TypeVar must have at least two constrained types"*), but it still seems potentially dangerous to conflate these two potential meanings. So maybe we should change the return type to `Option<&[Type<'db>]>` and return `None` here?

---

_@sharkdp reviewed on 2024-11-08 09:32_

---

_@sharkdp reviewed on 2024-11-08 09:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:1381 on 2024-11-08 09:34_

Should we raise a diagnostic here if there are fewer than two elements (see also my other comment)?

---

_@sharkdp reviewed on 2024-11-08 09:49_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/generics.md`:71 on 2024-11-08 09:49_

Similar to my other two comments: having this be the empty tuple is a bit confusing. What pyright infers for the unconstrained case seems more sensible to me: `tuple[Any, ...]`. I realize that this would require #13855, which in turn requires generics … which requires this PR.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/generics.md`:105 on 2024-11-08 11:12_

nit

```suggestion
class A: ...
class B: ...
class A1(A): ...
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1639 on 2024-11-08 11:15_

Strictly-speaking, I think `NoDefault` does not need to be part of this enum -- we could represent its singletonness by special-casing `KnownClass::NoDefaultType` in the same way as we do with `NoneType` here:

https://github.com/astral-sh/ruff/blob/fbf140a665629ce31191e56918bec6a724a24617/crates/red_knot_python_semantic/src/types.rs#L903-L906

However, I think the way you have it is clearer to understand; possibly it would be better to add `None` to this enum as well and get rid of the special-casing in `Type::is_singleton`.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:496 on 2024-11-08 11:17_

This surprises me -- I assumed these would be `DeclarationAndBinding`

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1387 on 2024-11-08 11:20_

You can `.collect()` directly into a boxed slice:

```suggestion
               let tys: Box<_> = elts
                    .iter()
                    .map(|expr| self.infer_type_expression(expr))
                    .collect();
                let constraints = TypeVarBoundOrConstraints::Constraints(tys.clone());
                self.store_expression_type(expr, Type::Tuple(TupleType::new(self.db, tys)));

```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1778 on 2024-11-08 11:25_

If I understand correctly, I think the design of this struct means that two TypeVars with the same `{name, constraints, bound, default}` will compare and hash equal in our internal representation. I think that might cause problems for us in the medium/long run. Mypy has had a _lot_ of problems over the years with ensuring that two similarly named TypeVars are not accidentally treated as equivalent types; I believe this is something that pyright has had bugs with as well.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1671 on 2024-11-08 11:30_

I guess I'm curious what including `TypeVar` in this struct gives us, relative to having a dedicated `Type::TypeParameter(TypeParameterType)` variant and an enum like this:

```rs
enum TypeParameterType {
    TypeVar(TypeVarInstance<'db>),
    ParamSpec(ParamSpecInstance<'db>),
    TypeVarTuple(TypeVarTupleInstance<'db>),
}
```

I can see that `TypeVar` instances are similar to `KnownInstance`s in that they have a fallback to a regular `Instance` type for many operations. But they differ in that there is only one `typing.Literal` symbol and only one `typing.NoDefault` symbol, whereas there are many `TypeVar` instances.

You've already had to add a significant amount of special casing for the `TypeVar` variant of this enum in the `KnownInstanceType::member` method, and I think as we build out support for type variables across our data model the differences to the other variants of this enum will only continue to grow.

---

_@AlexWaygood reviewed on 2024-11-08 11:31_

---

_@MichaReiser reviewed on 2024-11-08 12:30_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1778 on 2024-11-08 12:30_

If that's the case, then we should use `salsa::tracked` here so that equality is defined by its identity.

---

_@carljm reviewed on 2024-11-08 15:05_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics.md`:71 on 2024-11-08 15:05_

In this case, we are simply precisely typing the actual runtime value of `T.__constraints__`. The runtime does, in fact, return an empty tuple for the `__constraints__` property of a typevar that does not have constraints. Your argument may be an argument for why the runtime should preferably not do that, but given that it does, I don't see a case for typing it less precisely than we are able to.

I don't think the type we infer for this attribute of the typevar object has much relevance to how we actually use the typevar constraints internally; it's purely for the sake of user code that introspects a typevar object (which is quite rare in practice.) So changing our internal representation may have benefits in keeping our internal semantics clearer, but I think changing this inference would be pretty much irrelevant to keeping our internal semantics clear.

---

_@carljm reviewed on 2024-11-08 15:20_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1671 on 2024-11-08 15:20_

> I think as we build out support for type variables across our data model the differences to the other variants of this enum will only continue to grow.

I think this is the point that I don't believe is true, which gives me a different perspective on this tradeoff.

When we support typevars as annotations, we will need to add a representation for "something typed by a type variable", e.g. `x: T`. This is a very special type, and we will definitely need a dedicated `Type` variant for it. It certainly will not reuse the type of the type variable object itself (though it will likely internally store a reference to the same `TypeVarInstance`.) The type of the type variable itself is a singleton type representing exactly one instance of `typing.TypeVar`, whereas the type of `x` in `x: T` is the type "some unknown set of objects, constrained by the upper bound or constraints on the type var." Reusing the same type variant for these would be similar to trying to share the same `Type` variant for `Type::ClassLiteral` and `Type::Instance` -- they are too semantically different for that to make sense.

The type of the type variable itself, as a runtime object, which is what we have in this PR, is (I believe) feature-complete as of this PR, and shouldn't need to change or expand (though of course we will need to add paramspecs and typevartuples). It is nothing more than a known instance with some attributes whose type we can infer more precisely than typeshed. (We probably don't even _need_ to do that, it was just convenient for testing purposes.) Typevar objects don't have much interesting behavior as runtime objects, so I don't see what else we would need to add in future.

I think it's quite a significant benefit to use `KnownInstance` for this type to get the fallback behavior for free, and avoid the need to add another variant, with very similar behavior to `KnownInstance`, to every Type match. (And in the end, add _two_ new `Type` variants for typevars.)

> But they differ in that there is only one `typing.Literal` symbol and only one `typing.NoDefault` symbol, whereas there are many `TypeVar` instances.

There is exactly one instance of each `TypeVar`, though. Which is why the `TypeVar` variant holds data to describe _which_ typevar it represents, but the `typing.NoDefault` and `typing.Literal` variants don't. It is still the case that every given `Type::KnownInstance` represents exactly one singleton object.

---

_@AlexWaygood reviewed on 2024-11-08 16:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1671 on 2024-11-08 16:18_

Ahhh, I see. Okay, this makes a lot more sense. Please add a doc-comment to the `TypeVarInstance` struct clarifying this, though! I did indeed misunderstand the purpose of this variant and that struct when reading through the PR.

---

_@carljm reviewed on 2024-11-08 17:02_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1758 on 2024-11-08 17:02_

Yes, makes sense; done.

---

_@AlexWaygood reviewed on 2024-11-08 17:04_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1778 on 2024-11-08 17:04_

I think the visibility of a lot of things in the `types` submodule is kind of chaotic right now 😆

---

_@carljm reviewed on 2024-11-08 17:06_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1778 on 2024-11-08 17:06_

I left `bound_or_constraints` private because it should be accessed via the `upper_bound` and `constraints` methods instead. The discrepancy between `name` and `default_ty` was accidental.

I agree we may as well keep them all private until we need them to be otherwise; done!

---

_@carljm reviewed on 2024-11-08 17:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1715 on 2024-11-08 17:07_

Good call, thanks. (This might make a nice clippy rule?)

---

_@carljm reviewed on 2024-11-08 18:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1401 on 2024-11-08 18:22_

Ah yeah, thank you! I had a vague feeling there was a better way here.

---

_@carljm reviewed on 2024-11-08 18:33_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1383 on 2024-11-08 18:33_

Yeah, that's definitely better. I thought it was slightly semantically odd to store the constraints as a `TupleType`, but I don't think there's actually any problem with it; they are provided syntactically as a tuple, and they are just a list of types, which is what a `TupleType` also is. And it's definitely more efficient.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1744 on 2024-11-08 18:37_

I don't think there's anything meta about this data, is there? :P

```suggestion
/// Data regarding a single type variable.
///
/// This is referenced by `KnownInstanceType::TypeVar` (to represent the singleton type of the
/// runtime `typing.TypeVar` object itself). In the future, it will also be referenced by a new
/// `Type` variant to represent the type that this typevar represents as an annotation (that is, an
/// unknown set of objects, constrained by the upper-bound/constraints on this type var, defaulting
/// to the default type of this type var when not otherwise bound).
```

---

_@carljm reviewed on 2024-11-08 18:38_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1401 on 2024-11-08 18:38_

It looks like if I turn `TypeVarInstance` into a tracked struct instead of an interned one, this doesn't work anymore.

---

_@AlexWaygood reviewed on 2024-11-08 18:43_

---

_@carljm reviewed on 2024-11-08 19:50_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1778 on 2024-11-08 19:50_

Great catch! Fixed to use `salsa::tracked` and added a comment about why this matters.

Discovered a salsa requirement in the process: any field of a tracked struct which has a `'db` lifetime must itself be a tracked or interned Salsa struct, or must derive `salsa::Update`. This is not a hard requirement to abide by, but the error if you fail to is quite opaque: it's just an error on the tracked struct definition claiming that lifetimes don't live long enough, `'db` must outlive `'static`.

---

_@AlexWaygood reviewed on 2024-11-08 19:53_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1744 on 2024-11-08 19:53_

closing parentheses should come before the period unless it's a full sentence inside the parentheses ;)

```suggestion
/// defaulting to the default type of this type var when not otherwise bound to a type).
```

---

_@AlexWaygood approved on 2024-11-08 19:54_

Thank you!

---

_@carljm reviewed on 2024-11-08 20:05_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:496 on 2024-11-08 20:05_

I don't think making them declarations would have any effect.

They are "declared" in their own scope, and then used in the function or class body scope, which is a nested scope. There's no prohibition against redefining a declared name from an outer scope; you're just shadowing it with your own local, which is allowed. So making them declarations doesn't actually prevent you from arbitrarily shadowing the typevar name with something totally different inside the function/class body.

The only thing it would prevent is reassigning the name _in the type param scope_. But this isn't possible anyway, because type param scopes can't contain statements or named expressions.

Later we'll also want to support typevars of the form `T = typing.TypeVar("T")`. But these won't even be `DefinitionKind::TypeVar`, they'll just be `DefinitionKind::Assignment`. And since we won't know until type inference that the RHS even is a typevar, I don't think we could special-case those to be declarations even if we wanted to (and I don't think we do; I think declaration-or-not should be determined purely by the syntactic form, not by the type of the RHS of an assignment.)

If we decide we want to emit an error if you shadow a type variable, we can certainly do that (but again I'm not sure we should; neither pyright nor mypy does), but it would be a special-case diagnostic, it can't be done by making type parameters declarations.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1639 on 2024-11-08 20:44_

Great observation! After playing with it, I came to feel the opposite; I think for a singleton type it's simpler if we just represent its instance as `Type::Instance(KnownClass)`, and reserve `KnownInstance` for cases where we actually need to distinguish between different instances of one type. The extra case in `Type::is_singleton` is not really a burden, and we could even add a `KnownClass::is_singleton` to better consolidate this information into `KnownClass`.

So I removed `KnownInstanceType::NoDefault`, and added `KnownClass::NoDefaultType` to the existing check for `NoneType` in `Type::is_singleton`.

---

_@carljm reviewed on 2024-11-08 20:44_

---

_@AlexWaygood reviewed on 2024-11-08 20:53_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1639 on 2024-11-08 20:53_

> and we could even add a `KnownClass::is_singleton` to better consolidate this information into `KnownClass`.

I like that idea quite a lot, actually! Though no need to do it in this PR if you don't want to

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:496 on 2024-11-08 20:55_

(I did end up deciding to make them declarations anyway -- even if it makes no difference, I think it makes semantic sense, and actually `DeclarationAndBinding` is marginally more efficient since it doesn't have to look for other reaching declarations.)

---

_@carljm reviewed on 2024-11-08 20:55_

---

_@carljm reviewed on 2024-11-08 21:03_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1639 on 2024-11-08 21:03_

Done!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1578 on 2024-11-08 21:08_

```suggestion
    const fn is_singleton(self) -> bool {
```

---

_@AlexWaygood reviewed on 2024-11-08 21:08_

---

_@carljm reviewed on 2024-11-08 21:18_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1381 on 2024-11-08 21:18_

Yes, we should! Done.

---

_Merged by @carljm on 2024-11-08 21:23_

---

_Closed by @carljm on 2024-11-08 21:23_

---

_Branch deleted on 2024-11-08 21:23_

---

_@carljm reviewed on 2024-11-08 21:23_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1578 on 2024-11-08 21:23_

Oops, already had hit the auto-merge button. I'll push a follow-up for this.

---

_@carljm reviewed on 2024-11-08 21:25_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1578 on 2024-11-08 21:25_

https://github.com/astral-sh/ruff/pull/14211

---

_@AlexWaygood reviewed on 2024-11-08 22:24_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:675 on 2024-11-08 22:24_

I think this makes sense, because I'm guessing `NoDefaultType` sometimes comes from `typing` and sometimes comes from `typing_extensions`. But it doesn't actually seem to be tested anywhere; no tests fail if I revert this change.

I also wonder if this and some code in `display.rs` could be micro-optimised slightly by reducing some Salsa lookups:

```diff
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -666,13 +666,11 @@ impl<'db> Type<'db> {
                     Type::Instance(InstanceType { class: self_class }),
                     Type::Instance(InstanceType { class: target_class })
                 )
-                if (
-                    self_class.is_known(db, KnownClass::NoneType) &&
-                    target_class.is_known(db, KnownClass::NoneType)
-                ) || (
-                    self_class.is_known(db, KnownClass::NoDefaultType) &&
-                    target_class.is_known(db, KnownClass::NoDefaultType)
-                )
+                if {
+                    let self_class = self_class.known(db);
+                    matches!(self_class, Some(KnownClass::NoDefaultType | KnownClass::NoneType))
+                        && self_class == target_class.known(db)
+                }
             )
     }
 
diff --git a/crates/red_knot_python_semantic/src/types/display.rs b/crates/red_knot_python_semantic/src/types/display.rs
index 5bcafd902..bb0603ed0 100644
--- a/crates/red_knot_python_semantic/src/types/display.rs
+++ b/crates/red_knot_python_semantic/src/types/display.rs
@@ -66,15 +66,13 @@ impl Display for DisplayRepresentation<'_> {
             Type::Any => f.write_str("Any"),
             Type::Never => f.write_str("Never"),
             Type::Unknown => f.write_str("Unknown"),
-            Type::Instance(InstanceType { class })
-                if class.is_known(self.db, KnownClass::NoneType) =>
-            {
-                f.write_str("None")
-            }
-            Type::Instance(InstanceType { class })
-                if class.is_known(self.db, KnownClass::NoDefaultType) =>
-            {
-                f.write_str("NoDefault")
+            Type::Instance(InstanceType { class }) => {
+                let representation = match class.known(self.db) {
+                    Some(KnownClass::NoDefaultType) => "NoDefault",
+                    Some(KnownClass::NoneType) => "None",
+                    _ => class.name(self.db),
+                };
+                f.write_str(representation)
             }
             // `[Type::Todo]`'s display should be explicit that is not a valid display of
             // any other type
@@ -87,7 +85,6 @@ impl Display for DisplayRepresentation<'_> {
             Type::SubclassOf(SubclassOfType { class }) => {
                 write!(f, "type[{}]", class.name(self.db))
             }
-            Type::Instance(InstanceType { class }) => f.write_str(class.name(self.db)),
             Type::KnownInstance(known_instance) => f.write_str(known_instance.as_str()),
             Type::FunctionLiteral(function) => f.write_str(function.name(self.db)),
             Type::Union(union) => union.display(self.db).fmt(f),
```

---

_@AlexWaygood reviewed on 2024-11-08 22:46_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:914 on 2024-11-08 22:46_

nit

```suggestion
                class.known(db).is_some_and(|known_class| known_class.is_singleton())
```

---

_@carljm reviewed on 2024-11-09 19:06_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:675 on 2024-11-09 19:06_

https://github.com/astral-sh/ruff/pull/14232

---

_@carljm reviewed on 2024-11-09 19:06_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:914 on 2024-11-09 19:06_

https://github.com/astral-sh/ruff/pull/14232

---
