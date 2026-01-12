```yaml
number: 14876
title: "Handle type[Any] correctly"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/type-of-any
created_at: 2024-12-09T15:16:32Z
updated_at: 2024-12-10T21:12:40Z
url: https://github.com/astral-sh/ruff/pull/14876
synced_at: 2026-01-12T15:55:49Z
```

# Handle type[Any] correctly

---

_@dcreager_

This adds support for `type[Any]`, which represents an unknown type (not an instance of an unknown type), and `type`, which we are choosing to interpret as `type[object]`.

Closes #14546 

---

_Label `red-knot` added by @AlexWaygood on 2024-12-09 15:18_

---

_Comment by @github-actions[bot] on 2024-12-09 15:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dcreager on 2024-12-09 19:45_

---

_Review requested from @carljm by @dcreager on 2024-12-09 19:45_

---

_Review requested from @MichaReiser by @dcreager on 2024-12-09 19:45_

---

_Review requested from @AlexWaygood by @dcreager on 2024-12-09 19:45_

---

_Review requested from @sharkdp by @dcreager on 2024-12-09 19:45_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_of/any.md`:45 on 2024-12-09 20:53_

```suggestion
    # TODO bound method types
    reveal_type(x.__repr__)  # revealed: Literal[__repr__]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_of/any.md`:9 on 2024-12-09 20:59_

I think this could actually be `Literal[__repr__] & Any` -- it could be a subclass of `type` with a narrower definition of `__repr__`, but it can't be wider than `object.__repr__`. (Though as discussed below, the `Literal[__repr__]` part of this isn't really correct, we just don't handle bound methods yet.) But I think all of this can be TODO pending more handling of `Type::SubclassOf` attributes.

```suggestion
    # TODO could be `<object.__repr__ type> & Any`
    reveal_type(x.__repr__)  # revealed: Any
```

---

_@carljm approved on 2024-12-09 20:59_

🚀 

---

_@sharkdp reviewed on 2024-12-09 21:22_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_of/any.md`:14 on 2024-12-09 21:22_

Note that we do not have call-signature checking, so this doesn't test anything yet. If you want to check for assignability, you would have to do something like
```suggestion
x: type[Any] = object
x: type[Any] = type
x: type[Any] = A
```
For some reason, those assignments currently seem to fail(?).

---

_@carljm reviewed on 2024-12-09 21:23_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_of/any.md`:14 on 2024-12-09 21:23_

Oh, great catch! Yeah, if those assignments are failing, we should definitely understand why.

---

_@sharkdp reviewed on 2024-12-09 21:31_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/mro.rs`:345 on 2024-12-09 21:31_

This seems to be unused?

---

_@sharkdp reviewed on 2024-12-09 22:06_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_of/any.md`:14 on 2024-12-09 22:06_

I think assignability falls back to subtyping, and `type[Any]` does not participate in subtyping. We probably need something like this special case in `Type::is_assignable_to`:
```rs
(Type::ClassLiteral(_) | Type::SubclassOf(_), Type::SubclassOf(target))
    if target.base == ClassBase::Any =>
{
    true
}
```

It might make sense to add a new `Ty::SubclassOfAny` (and `Ty::SubclassOf(…)`) variant to our testing-enum `Ty` and to write a few explicit unit tests for this type?

For example, I think we currently treat `type[Any]` as fully static.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_of/any.md`:14 on 2024-12-10 14:54_

Thanks for catching that, @sharkdp!

I had to add a few more clauses to `is_assignable_to`.  I added test cases for all of the entries in this truth table:

| | `Instance(type)` | `SubtypeOf(Any)` | `SubtypeOf(object)` | `SubtypeOf(str)` |
|-|-|-|-|-|
|`Instance(type)`|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:||
|`SubtypeOf(Any)`|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|`SubtypeOf(object)`|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:||
|`SubtypeOf(str)`|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|

If I'm understanding the semantics, `Instance(type)` and `SubtypeOf(object)` should be equivalent to each other, and those are not assignable to `SubtypeOf(str)`.  Those are the only two cases of non-assignability in the table.  `SubtypeOf(Any)`, in particular, should be assignable to any `type`-like type.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/mro.rs`:345 on 2024-12-10 14:56_

Removed

I had added it thinking that I'd use it in `Type::is_assignable_to`, but then I didn't — largely because I also need to handle assignability with `Instance(type)`, and it seemed better to have all of the cases in a single place.  

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_of/any.md`:45 on 2024-12-10 14:57_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_of/any.md`:9 on 2024-12-10 14:58_

Done

---

_@dcreager reviewed on 2024-12-10 14:59_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_of/any.md`:17 on 2024-12-10 15:27_

It makes me slightly nervous that all the assignment tests in this file are "no error" assignments; we could make all these tests pass if `type[...]` were always just `Any`! (Not the `reveal_type` ones, but the assignment ones.) We could add something like this to show that we do actually discriminate?
```suggestion
x: type[Any] = A()  # error: [invalid-assignment]
```
And maybe similar in below sections, too?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_of/any.md`:30 on 2024-12-10 15:29_

This section is supposed to be about bare `type`, right?
```suggestion
x: type = object
x: type = type
x: type = A
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_of/any.md`:24 on 2024-12-10 15:32_

Per discussion this morning in Discord:
```suggestion
## Bare type

The interpretation of bare `type` is not clear: existing wording in the spec does not match the behavior of mypy or pyright. For now we interpret it as simply "an instance of `builtins.type`", which is equivalent to `type[object]`. This is similar to the current behavior of mypy, and pyright in strict mode.

```py
def f(x: type):
    reveal_type(x)  # revealed: type
    # TODO bound method types
    reveal_type(x.__repr__)  # revealed: Literal[__repr__]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_of/any.md`:45 on 2024-12-10 15:33_

This section is supposed to be about `type[object]`?
```suggestion
x: type[object] = object
x: type[object] = type
x: type[object] = A
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1724 on 2024-12-10 15:34_

Per discussion in Discord:
```suggestion
                Type::subclass_of(KnownClass::Object.to_class())
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3398 on 2024-12-10 15:36_

Can we also add this one as a test case for `is_equivalent_to`? (Should also require adding the case to `is_equivalent_to`.)

---

_@carljm reviewed on 2024-12-10 15:47_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_of/any.md`:30 on 2024-12-10 15:53_

Yes, yes it is!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_of/any.md`:45 on 2024-12-10 15:53_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_of/any.md`:17 on 2024-12-10 15:54_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:1724 on 2024-12-10 15:55_

Do you have a preference between that and `Type::instance(KnownClass::Type.to_class())`?

Or we had also discussed consolidating those two down into a single internal representation.  I don't think that would be (easily) expressible in the Rust types, so I'd consider adding a `debug_assert!` in the other constructor function to help ensure we only ever instantiate the preferred variant.

---

_@dcreager reviewed on 2024-12-10 15:58_

(Initial responses, still need to make the `type ⇒ type[object]` change and add the `equivalent_to` tests)

---

_@AlexWaygood reviewed on 2024-12-10 16:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1724 on 2024-12-10 16:01_

I think I would expect `Type::SubclassOf(object)` to delegate to `Type::Instance(type)` in most respects; as such, it feels like `Type::Instance(type)` is a "simpler", "more fundamental" representation, so I'd probably go with that where have the choice between the two.

Not sure whether or not it's worth forbidding the instantiation of `Type::SubclassOf(object)` as a type; if we do everything correctly, it should behave identically to `Type::Instance(type)`, so I don't think it's _necessary_

---

_@carljm reviewed on 2024-12-10 16:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1724 on 2024-12-10 16:01_

Oh, yeah, now that you mention it, I think the instance type is better.

Until we have a clear reason to do so, I think we don't need to worry about consolidating to a single representation; we can have both, and they will display differently (`type` vs `type[object]`), but we should treat them as equivalent (subtypes of each other).

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3310 on 2024-12-10 16:20_

nit: looking at the implementation of the tests that use this variant, maybe it should be called `SubclassOfBuiltinClass`?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/mro.rs`:337 on 2024-12-10 16:25_

it looks like this is only used in one place. I'm not sure I love having this method -- a `ClassBase` is not the same concept as a `Type`, but having this method feels like it blurs the lines between the two enums in a slightly unfortunate way. Can we just inline it?

```diff
diff --git a/crates/red_knot_python_semantic/src/types.rs b/crates/red_knot_python_semantic/src/types.rs
index defa2dd9a..d49a67df3 100644
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -666,9 +666,10 @@ impl<'db> Type<'db> {
             {
                 self_class.class.is_subclass_of_base(db, target_class.base)
             }
-            (Type::SubclassOf(self_class), Type::SubclassOf(target_class)) => {
-                self_class.base.is_subtype_of(db, target_class.base)
-            }
+            (
+                Type::SubclassOf(SubclassOfType { base: self_base }),
+                Type::SubclassOf(SubclassOfType { base: target_base }),
+            ) => Type::from(self_base).is_subtype_of(db, Type::from(target_base)),
             (
                 Type::SubclassOf(SubclassOfType {
                     base: ClassBase::Class(self_class),
diff --git a/crates/red_knot_python_semantic/src/types/mro.rs b/crates/red_knot_python_semantic/src/types/mro.rs
index 75da7f7f3..131b512ed 100644
--- a/crates/red_knot_python_semantic/src/types/mro.rs
+++ b/crates/red_knot_python_semantic/src/types/mro.rs
@@ -328,14 +328,6 @@ impl<'db> ClassBase<'db> {
         Display { base: self, db }
     }
 
-    pub fn is_subtype_of(self, db: &'db dyn Db, target: ClassBase<'db>) -> bool {
-        match (self, target) {
-            (ClassBase::Any | ClassBase::Todo | ClassBase::Unknown, _) => false,
-            (_, ClassBase::Any | ClassBase::Todo | ClassBase::Unknown) => false,
-            (ClassBase::Class(class), ClassBase::Class(target)) => class.is_subclass_of(db, target),
-        }
-    }
-
     /// Return a `ClassBase` representing the class `builtins.object`
```

---

_@AlexWaygood reviewed on 2024-12-10 16:27_

Nice!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/mro.rs`:337 on 2024-12-10 17:01_

That's much nicer, though I had to use `Class::is_subclass_of` instead of `Type::is_subtype_of`, since `ClassLiteral(C)` ⊈ `ClassLiteral(D)` even if C is a subclass of D.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:3310 on 2024-12-10 17:01_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:3398 on 2024-12-10 17:44_

Done.  I also took the liberty of updating the test to verify that equivalence is symmetric.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:1724 on 2024-12-10 17:47_

Done.  Interpreting `type` as `type[object]` means we don't need this special-case clause anymore, since interpreting it as `Instance(type)` is what we already do for any other class literal.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_of/any.md`:24 on 2024-12-10 17:50_

Done, though the revealed type of `x.__repr__` now becomes a TODO for instance attributes

---

_@dcreager reviewed on 2024-12-10 18:02_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:611 on 2024-12-10 18:07_

I removed these since they (and the new `Subclass(Any)` form) are handled by the `is_fully_static` checks above.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:658 on 2024-12-10 18:07_

This is just a special case of the metaclass check below, which I've extended to work for `ClassLiteral(C)` as well as `SubclassOf(C)`

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:1099 on 2024-12-10 18:09_

`SubclassOf(Any)` is not fully static

---

_@dcreager reviewed on 2024-12-10 18:09_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1099 on 2024-12-10 18:13_

There are unit tests below for `is_fully_static` as well; let's add a couple tests (negative and positive) for this case?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:835 on 2024-12-10 18:18_

this gets a little hard to read 😄 how about writing it like this?

```suggestion
            (Type::Instance(self_instance), Type::Instance(target_instance)) => {
                let Some(self_known_class) = self_instance.class.known(db) else {
                    return false;
                };
                if !matches!(
                    self_known_class,
                    KnownClass::NoneType | KnownClass::NoDefaultType
                ) {
                    return false;
                }
                Some(self_known_class) == target_instance.class.known(db)
            }
```

---

_@carljm reviewed on 2024-12-10 18:19_

Looking good! In addition to the additional unit tests I mentioned inline, this PR seems a good candidate to run the quickcheck property-based tests for core type algebra invariants. These don't run in CI because they are slow, but it would be good to see whether they catch any issues here. See https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/src/types/property_tests.rs

EDIT: just realized that in order to get value from that on this PR, we should also add SubclassOf types to the `arbitrary_core_type` function there!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:851 on 2024-12-10 18:20_

```suggestion
            ) => {
                object_class.is_known(db, KnownClass::Object)
                    && type_class.is_known(db, KnownClass::Type)
            }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:678 on 2024-12-10 18:21_

I think I'd either call the captured variable here `target_instance` or do something like this

```suggestion
                Type::Instance(InstanceType {class: target_class}),
```

so that you can avoid the awkward `target_class.class` immediately below

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:657 on 2024-12-10 18:23_

similarly here, maybe the captured variables inside `Type::SubclassOf` could be renamed `target_ty` rather than `target_class`, since the inner data wrapped by this variant is no longer a class

---

_@AlexWaygood reviewed on 2024-12-10 18:23_

---

_@carljm reviewed on 2024-12-10 18:26_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:835 on 2024-12-10 18:26_

We have to be careful here; these `return false` in the suggested change are not correct. They would have to be `return self == other`, and I'm not sure we want to duplicate that condition in various places.

---

_@carljm reviewed on 2024-12-10 18:27_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:851 on 2024-12-10 18:27_

This one should be safe because a `Type::Instance` can't be `==` to a `Type::SubclassOf`.

---

_@AlexWaygood reviewed on 2024-12-10 18:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:835 on 2024-12-10 18:28_

argh, great spot. thanks.

---

_Comment by @sharkdp on 2024-12-10 19:34_

> In addition to the additional unit tests I mentioned inline, this PR seems a good candidate to run the quickcheck property-based tests for core type algebra invariants. These don't run in CI because they are slow, but it would be good to see whether they catch any issues here. See https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/src/types/property_tests.rs

I already did this. I didn't mention it here because they are currently broken (surprise!). I'll put up a small PR with a fix.

Edit: https://github.com/astral-sh/ruff/pull/14897

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:678 on 2024-12-10 20:02_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:657 on 2024-12-10 20:03_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:835 on 2024-12-10 20:05_

I had also considered handling each of the special cases with a separate `if` statement + early return.  Then it would possibly be clearer that `self == other` is the fall-back / general case.  But we had other large `match` blocks elsewhere that made me think that might be preferred house style.  I will take a stab at the early return pattern and see if it's more readable.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:1099 on 2024-12-10 20:08_

Done

---

_@dcreager reviewed on 2024-12-10 20:09_

---

_@sharkdp approved on 2024-12-10 20:16_

Thank you for all the updates and the new tests!

FWIW, I did successfully run the property tests on your branch after cherry-picking my fix in #14897 and applying the following patch, where the 4=>3 change is a weird way to suppress the creation of intersection types to prevent running into #14899:
```diff
diff --git a/crates/red_knot_python_semantic/src/types/property_tests.rs b/crates/red_knot_python_semantic/src/types/property_tests.rs
index 15f4d5648..e6b223034 100644
--- a/crates/red_knot_python_semantic/src/types/property_tests.rs
+++ b/crates/red_knot_python_semantic/src/types/property_tests.rs
@@ -64,6 +64,10 @@ fn arbitrary_core_type(g: &mut Gen) -> Ty {
         Ty::BuiltinClassLiteral("int"),
         Ty::BuiltinClassLiteral("bool"),
         Ty::BuiltinClassLiteral("object"),
+        Ty::SubclassOfAny,
+        Ty::SubclassOfBuiltinClass("object"),
+        Ty::SubclassOfBuiltinClass("int"),
+        Ty::SubclassOfBuiltinClass("bool"),
     ])
     .unwrap()
     .clone()
@@ -78,7 +82,7 @@ fn arbitrary_type(g: &mut Gen, size: u32) -> Ty {
     if size == 0 {
         arbitrary_core_type(g)
     } else {
-        match u32::arbitrary(g) % 4 {
+        match u32::arbitrary(g) % 3 {
             0 => arbitrary_core_type(g),
             1 => Ty::Union(
                 (0..*g.choose(&[2, 3]).unwrap())
```

---

_@dcreager reviewed on 2024-12-10 20:22_

> I did successfully run the property tests on your branch

Thanks @sharkdp, was just looking at the property tests myself and running into the `assignable` bug

---

_@dcreager reviewed on 2024-12-10 20:29_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:835 on 2024-12-10 20:29_

> I will take a stab at the early return pattern and see if it's more readable.

Now using early return.  lmkwyt

---

_@carljm approved on 2024-12-10 20:32_

Looks good to me!!

---

_Merged by @dcreager on 2024-12-10 21:12_

---

_Closed by @dcreager on 2024-12-10 21:12_

---

_Branch deleted on 2024-12-10 21:12_

---
