```yaml
number: 15103
title: "[red-knot] `knot_extensions` Python API"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/type-level-api
created_at: 2024-12-22T23:05:32Z
updated_at: 2025-01-08T21:24:34Z
url: https://github.com/astral-sh/ruff/pull/15103
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] `knot_extensions` Python API

---

_Pull request opened by @sharkdp on 2024-12-22 23:05_

## Summary

Adds a type-check-time Python API that allows us to create and manipulate types and to test various of their properties. For example, this can be used to write a Markdown test to make sure that `A & B` is a subtype of `A` and `B`, but not of an unrelated class `C` (something that requires quite a bit more code to do in Rust):
```py
from knot_extensions import Intersection, is_subtype_of, static_assert

class A: ...
class B: ...

type AB = Intersection[A, B]

static_assert(is_subtype_of(AB, A))
static_assert(is_subtype_of(AB, B))

class C: ...
static_assert(not is_subtype_of(AB, C))
```

I think this functionality is also helpful for interactive debugging sessions, in order to query various properties of Red Knot's type system. Which is something that otherwise requires a custom Rust unit test, some boilerplate code and constant re-compilation.

## Comparison Rust ↔ Python: `is_assignable_to` tests

<details>
  <summary>Rust</summary>

```rs
#[test_case(Ty::BuiltinInstance("str"), Ty::BuiltinInstance("object"))]
#[test_case(Ty::BuiltinInstance("int"), Ty::BuiltinInstance("object"))]
#[test_case(Ty::Unknown, Ty::IntLiteral(1))]
#[test_case(Ty::Any, Ty::IntLiteral(1))]
#[test_case(Ty::Never, Ty::IntLiteral(1))]
#[test_case(Ty::IntLiteral(1), Ty::Unknown)]
#[test_case(Ty::IntLiteral(1), Ty::Any)]
#[test_case(Ty::IntLiteral(1), Ty::BuiltinInstance("int"))]
#[test_case(Ty::StringLiteral("foo"), Ty::BuiltinInstance("str"))]
#[test_case(Ty::StringLiteral("foo"), Ty::LiteralString)]
#[test_case(Ty::LiteralString, Ty::BuiltinInstance("str"))]
#[test_case(Ty::BytesLiteral("foo"), Ty::BuiltinInstance("bytes"))]
#[test_case(Ty::IntLiteral(1), Ty::Union(vec![Ty::BuiltinInstance("int"), Ty::BuiltinInstance("str")]))]
#[test_case(Ty::IntLiteral(1), Ty::Union(vec![Ty::Unknown, Ty::BuiltinInstance("str")]))]
#[test_case(Ty::Union(vec![Ty::IntLiteral(1), Ty::IntLiteral(2)]), Ty::Union(vec![Ty::IntLiteral(1), Ty::IntLiteral(2)]))]
#[test_case(
    Ty::Union(vec![Ty::IntLiteral(1), Ty::IntLiteral(2)]),
    Ty::BuiltinInstance("int")
)]
#[test_case(
    Ty::Union(vec![Ty::IntLiteral(1), Ty::None]),
    Ty::Union(vec![Ty::BuiltinInstance("int"), Ty::None])
)]
#[test_case(Ty::Tuple(vec![Ty::Todo]), Ty::Tuple(vec![Ty::IntLiteral(2)]))]
#[test_case(Ty::Tuple(vec![Ty::IntLiteral(2)]), Ty::Tuple(vec![Ty::Todo]))]
#[test_case(Ty::SubclassOfAny, Ty::SubclassOfAny)]
#[test_case(Ty::SubclassOfAny, Ty::SubclassOfBuiltinClass("object"))]
#[test_case(Ty::SubclassOfAny, Ty::SubclassOfBuiltinClass("str"))]
#[test_case(Ty::SubclassOfAny, Ty::BuiltinInstance("type"))]
#[test_case(Ty::SubclassOfBuiltinClass("object"), Ty::SubclassOfAny)]
#[test_case(
    Ty::SubclassOfBuiltinClass("object"),
    Ty::SubclassOfBuiltinClass("object")
)]
#[test_case(Ty::SubclassOfBuiltinClass("object"), Ty::BuiltinInstance("type"))]
#[test_case(Ty::SubclassOfBuiltinClass("str"), Ty::SubclassOfAny)]
#[test_case(
    Ty::SubclassOfBuiltinClass("str"),
    Ty::SubclassOfBuiltinClass("object")
)]
#[test_case(Ty::SubclassOfBuiltinClass("str"), Ty::SubclassOfBuiltinClass("str"))]
#[test_case(Ty::SubclassOfBuiltinClass("str"), Ty::BuiltinInstance("type"))]
#[test_case(Ty::BuiltinInstance("type"), Ty::SubclassOfAny)]
#[test_case(Ty::BuiltinInstance("type"), Ty::SubclassOfBuiltinClass("object"))]
#[test_case(Ty::BuiltinInstance("type"), Ty::BuiltinInstance("type"))]
#[test_case(Ty::BuiltinClassLiteral("str"), Ty::SubclassOfAny)]
#[test_case(Ty::SubclassOfBuiltinClass("str"), Ty::SubclassOfUnknown)]
#[test_case(Ty::SubclassOfUnknown, Ty::SubclassOfBuiltinClass("str"))]
#[test_case(Ty::SubclassOfAny, Ty::AbcInstance("ABCMeta"))]
#[test_case(Ty::SubclassOfUnknown, Ty::AbcInstance("ABCMeta"))]
fn is_assignable_to(from: Ty, to: Ty) {
    let db = setup_db();
    assert!(from.into_type(&db).is_assignable_to(&db, to.into_type(&db)));
}

#[test_case(Ty::BuiltinInstance("object"), Ty::BuiltinInstance("int"))]
#[test_case(Ty::IntLiteral(1), Ty::BuiltinInstance("str"))]
#[test_case(Ty::BuiltinInstance("int"), Ty::BuiltinInstance("str"))]
#[test_case(Ty::BuiltinInstance("int"), Ty::IntLiteral(1))]
#[test_case(
    Ty::Union(vec![Ty::IntLiteral(1), Ty::None]),
    Ty::BuiltinInstance("int")
)]
#[test_case(
    Ty::Union(vec![Ty::IntLiteral(1), Ty::None]),
    Ty::Union(vec![Ty::BuiltinInstance("str"), Ty::None])
)]
#[test_case(
    Ty::SubclassOfBuiltinClass("object"),
    Ty::SubclassOfBuiltinClass("str")
)]
#[test_case(Ty::BuiltinInstance("type"), Ty::SubclassOfBuiltinClass("str"))]
fn is_not_assignable_to(from: Ty, to: Ty) {
    let db = setup_db();
    assert!(!from.into_type(&db).is_assignable_to(&db, to.into_type(&db)));
}
```

</details>

<details>
  <summary>Python (grouped by topic instead of assignable/not-assignable)</summary>

```py
from knot_extensions import static_assert, is_assignable_to, Unknown, TypeOf
from typing_extensions import Never, Any, Literal, LiteralString
from abc import ABCMeta

static_assert(is_assignable_to(str, object))
static_assert(is_assignable_to(int, object))
static_assert(not is_assignable_to(object, int))
static_assert(not is_assignable_to(int, str))

static_assert(is_assignable_to(Unknown, Literal[1]))
static_assert(is_assignable_to(Any, Literal[1]))
static_assert(is_assignable_to(Never, Literal[1]))
static_assert(is_assignable_to(Literal[1], Unknown))
static_assert(is_assignable_to(Literal[1], Any))
static_assert(is_assignable_to(Literal[1], int))
static_assert(not is_assignable_to(Literal[1], str))
static_assert(not is_assignable_to(int, Literal[1]))

static_assert(is_assignable_to(Literal["foo"], str))
static_assert(is_assignable_to(Literal["foo"], LiteralString))
static_assert(is_assignable_to(LiteralString, str))
static_assert(is_assignable_to(Literal[b"foo"], bytes))

static_assert(is_assignable_to(Literal[1], int | str))
static_assert(is_assignable_to(Literal[1], Unknown | str))
static_assert(is_assignable_to(Literal[1] | Literal[2], Literal[1] | Literal[2]))
static_assert(is_assignable_to(Literal[1] | Literal[2], int))
static_assert(is_assignable_to(Literal[1] | None, int | None))
static_assert(not is_assignable_to(Literal[1] | None, int))
static_assert(not is_assignable_to(Literal[1] | None, str | None))

static_assert(is_assignable_to(type[Any], type[Any]))
static_assert(is_assignable_to(type[Any], type[object]))
static_assert(is_assignable_to(type[Any], type[str]))
static_assert(is_assignable_to(type[object], type[Any]))
static_assert(is_assignable_to(type[object], type[object]))
static_assert(is_assignable_to(type[object], type))
static_assert(is_assignable_to(type[str], type[Any]))
static_assert(is_assignable_to(type[str], type[object]))
static_assert(not is_assignable_to(type[object], type[str]))
static_assert(is_assignable_to(type[str], type[str]))
static_assert(is_assignable_to(type[str], type))
static_assert(not is_assignable_to(type, type[str]))
static_assert(is_assignable_to(type, type[Any]))
static_assert(is_assignable_to(type, type[object]))
static_assert(is_assignable_to(type, type))
static_assert(is_assignable_to(TypeOf[str], type[Any]))
static_assert(is_assignable_to(type[str], type[Unknown]))
static_assert(is_assignable_to(type[Unknown], type[str]))
static_assert(is_assignable_to(type[Any], type[Unknown]))
static_assert(is_assignable_to(type[Any], ABCMeta))
static_assert(is_assignable_to(type[Unknown], ABCMeta))
```
</details>

## Other ideas

- [x] Introduce `assert_true(…)` to replace `reveal_type(…)  # revealed: Literal[True]`
- [x] Introduce `knot_extensions.Unknown` as a spelling for the `Unknown` type.
- [ ] ~~Should we have a way to spell the empty intersection type? Like `Intersection[()]`?~~ It's probably okay not to support this. Empty `Union`s or `Union`s with a single type are not supported either (even if they would have a well defined meaning).
- [ ] Support [`typing.assert_type`](https://docs.python.org/3/library/typing.html#typing.assert_type), and maybe alias it as `knot_extensions.assert_type`

## To do

- [x] Proper error handling
- [x] Put the stubs for the `knot_extensions` module in a dedicated package
- [x] Think about whether or not `TypeOf` is needed. Maybe introduce something like `knot_extensions.ClassLiteral[…]` instead for that specific purpose?

## Test Plan

New Markdown tests

---

_Label `red-knot` added by @sharkdp on 2024-12-22 23:05_

---

_Comment by @github-actions[bot] on 2024-12-22 23:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sharkdp reviewed on 2024-12-22 23:12_

---

_Review comment by @sharkdp on `crates/red_knot_vendored/vendor/typeshed/stdlib/VERSIONS`:344 on 2024-12-22 23:12_

We would obviously *not* put this in our vendored copy of `typeshed`. This was just the easiest option for this PoC.

---

_Review comment by @sharkdp on `crates/red_knot_vendored/vendor/typeshed/stdlib/red_knot.pyi`:19 on 2024-12-23 08:43_

These functions could potentially be annotated using https://discuss.python.org/t/pep-747-typeexpr-type-hint-for-a-type-expression/55984 (?)

---

_@sharkdp reviewed on 2024-12-23 08:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:3 on 2024-12-24 13:36_

nit: I'd probably call it `knot_extensions`, as it feels quite analogous to https://pypi.org/project/mypy-extensions/ and https://pypi.org/project/pyre-extensions/. (We could even consider publishing a `knot_extensions` package to PyPI... though with the current API, I'm not sure there'd be _much_ utility in doing so, since you'd likely never want to execute at runtime the snippets using this API -- they're most useful for static assertions.)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_api.rs`:107 on 2024-12-24 13:39_

nit: I think we actually don't need this one. `assert_false(is_subtype_of(X, Y))` can be written as `assert_true(not is_subtype_of(X, Y))`.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_api.rs`:46 on 2024-12-24 13:42_

nit: in most proposals for this feature (e.g. https://github.com/python/typing/issues/801), this is generally referred to as a `Not` type rather than a `Negate` type, so that would be the name I'd be most familiar with for this

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4210 on 2024-12-24 13:45_

nit: you can iterate directly over the tuple:

```suggestion
                        tuple
                            .iter()
                            .map(|element| self.infer_type_expression(element)),
```

---

_@AlexWaygood reviewed on 2024-12-24 13:47_

This is great! I'm impressed how simple the implementation is

---

_@sharkdp reviewed on 2025-01-02 08:17_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/type_api.rs`:46 on 2025-01-02 08:17_

Thanks!

---

_@sharkdp reviewed on 2025-01-03 09:17_

---

_Review comment by @sharkdp on `crates/red_knot_vendored/vendor/typeshed/stdlib/VERSIONS`:344 on 2025-01-03 09:17_

I tried to implement this, but it seems a bit involved. I'm opening this PR for review first, so we can discuss if we actually want this, before I invest more time.

---

_Marked ready for review by @sharkdp on 2025-01-03 09:18_

---

_Review requested from @carljm by @sharkdp on 2025-01-03 09:18_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-03 09:18_

---

_@sharkdp reviewed on 2025-01-03 09:59_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:3 on 2025-01-03 09:59_

Happy to change the name, if we decide to go forward with this.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:10 on 2025-01-03 10:35_

```suggestion
and `type[int]` to create new types. But some type-level operations that we rely on in Red Knot,
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:11 on 2025-01-03 10:35_

(There are active/dormant proposals for several of these to be standardised)

```suggestion
like intersections, cannot yet be expressed in Python. The `red_knot` module provides the
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:41 on 2025-01-03 10:40_

nit: `int` and `str` are not the best example here, because this happens at runtime:

```pycon
>>> class Foo(int, str): ...
... 
Traceback (most recent call last):
  File "<python-input-0>", line 1, in <module>
    class Foo(int, str): ...
TypeError: multiple bases have instance lay-out conflict
```

Ideally we'd detect this, and therefore infer that the intersection `int & str` is equivalent to `Never`. Achieving that might require us to special-case some builtin classes in Python, unfortunately (it has to do with the very special way that certain builtin classes are implemented at the C level). But it may still be worth doing, since subclassing a builtin isn't an uncommon thing in user code.

If/when we do start inferring `Never` for that intersection, presumably this example would break (and if it were naively fixed, it would no longer demonstrate what you're trying to demonstrate here).

TL;DR: it might be best to construct your own classes here rather than using builtins.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:67 on 2025-01-03 10:42_

Bikeshedding: I wonder if `static_assert` might be a better name than `assert_true`?

- `assert_true` doesn't feel like it differentiates the concept very much from the `assert` keyword at runtime, since `assert` statements at runtime also assert that a certain expression is true(thy).
- `static_assert` helps emphasise that the function only has an effect at type-checking time, not at runtime, and it's useful to be reminded of that when you're reading these mdtests

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2599 on 2025-01-03 10:48_

```suggestion
    RedKnotUnknown,
    // TODO: fill this enum out with more special forms, etc.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4201 on 2025-01-03 10:52_

Something like this seems a little simpler?

```diff
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index 98cd6ee4f..e6d1a1820 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -4198,14 +4198,14 @@ impl<'db> TypeInferenceBuilder<'db> {
         api_type: Type<'db>,
         arguments: &ast::Expr,
     ) -> Option<Type<'db>> {
-        match api_type.into_class_literal() {
-            Some(class)
-                if file_to_module(self.db(), class.class.file(self.db()))
+        match api_type {
+            Type::ClassLiteral(ClassLiteralType { class })
+                if file_to_module(self.db(), class.file(self.db()))
                     .is_some_and(|module| module.is_known(KnownModule::RedKnot)) =>
             {
                 let db = self.db();
 
-                let is_type_of = class.class.name(db).as_str() == "TypeOf";
+                let is_type_of = class.name(db).as_str() == "TypeOf";
 
                 let argument_types = match arguments {
                     ast::Expr::Tuple(tuple) => Either::Left(
@@ -4220,7 +4220,7 @@ impl<'db> TypeInferenceBuilder<'db> {
                     })),
                 };
 
-                let result = type_api::resolve_type_operation(db, class.class, argument_types);
+                let result = type_api::resolve_type_operation(db, class, argument_types);
 
                 match result {
                     Ok(ty) => ty,
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4208 on 2025-01-03 10:59_

You can simplify your handling of comparisons between `Name`s and `str`s, as `Name` implements `PartialEq<str>`:

```diff
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index 98cd6ee4f..617f07d58 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -4205,7 +4205,7 @@ impl<'db> TypeInferenceBuilder<'db> {
             {
                 let db = self.db();
 
-                let is_type_of = class.class.name(db).as_str() == "TypeOf";
+                let is_type_of = class.class.name(db) == "TypeOf";
 
                 let argument_types = match arguments {
                     ast::Expr::Tuple(tuple) => Either::Left(
@@ -4254,9 +4254,9 @@ impl<'db> TypeInferenceBuilder<'db> {
                 if file_to_module(self.db(), function.body_scope(self.db()).file(self.db()))
                     .is_some_and(|module| module.is_known(KnownModule::RedKnot)) =>
             {
-                let name = function.name(db).as_str();
+                let function_name = function.name(db);
 
-                let is_assertion = matches!(name, "assert_true");
+                let is_assertion = function_name == "assert_true";
 
                 let argument_types = arguments.args.iter().map(|arg| {
                     if is_assertion {
@@ -4266,11 +4266,7 @@ impl<'db> TypeInferenceBuilder<'db> {
                     }
                 });
 
-                let result = type_api::resolve_type_predicate(
-                    db,
-                    function.name(db).as_str(),
-                    argument_types,
-                );
+                let result = type_api::resolve_type_predicate(db, function_name, argument_types);
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4254 on 2025-01-03 11:00_

nit: you assigned `self.db()` to a variable just a couple of lines above ;)

```suggestion
                if file_to_module(db, function.body_scope(db).file(db))
```

---

_Review comment by @AlexWaygood on `crates/red_knot_vendored/vendor/typeshed/stdlib/VERSIONS`:344 on 2025-01-03 11:11_

Mypy has a solution for this for `mypy_extensions` that doesn't involve tinkering with typeshed's `VERSIONS` file. `mypy_extensions` exists as a runtime package published to PyPI, but the runtime package isn't `py.typed`. The type information for the package is published as a separate stubs package, `types-mypy-extensions`, and the source code for that stubs package is found in the [`stubs/mypy-extensions` directory](https://github.com/python/typeshed/tree/main/stubs/mypy-extensions) in typeshed.

The `stubs/` directory in typeshed confusingly does not contain all typeshed's stubs. It only contains typeshed's stubs for third-party packages outside the stdlib. Unlike typeshed's standard-library stubs, the stubs for these third-party packages in typeshed are not generally vendored by mypy (though pyright _does_ vendor them), as they're published as standalone packages to PyPI that you can `(uv) pip install` if you want your type checker to benefit from the type information provided by these packages. However, mypy makes an exception for the `mypy_extensions` package: it needs these types to always be available, and so it vendors just that package from typeshed's `stubs/` directory: https://github.com/python/mypy/tree/master/mypy/typeshed.

Is this relevant for us? Eh, after typing this all out, I'm not sure 🤷 We almost certainly don't want to contribute our stubs for this package to typeshed upstream just yet, so that would mean that we'd be "injecting" these stubs into a `stubs/knot_extensions` package at typeshed-sync time. This would involve a bunch of special casing in both our module resolver (we'd need to understand the `stubs/` directory) and our typeshed-sync GitHub workflow. So a solution where we just apply a patch to typeshed's `VERSIONS` file at typeshed-sync time might indeed be simplest for now...

---

_Review comment by @AlexWaygood on `crates/red_knot_vendored/vendor/typeshed/stdlib/red_knot.pyi`:5 on 2025-01-03 11:17_

I'm not sure how much this matters (since these APIs don't (yet?) exist at runtime), but I don't really love having `TypeOf`, `Not` and `Intersection` as generic classes in the stub. Although these APIs are similar to generic classes in that they accept type arguments, they're ultimately a pretty different concept to generic classes (they're special forms). If I were to add an implementation of these to the `typing` module at runtime, I definitely wouldn't implement them as classes, because it doesn't make sense to "create an instance of `Not`", nor does it make sense to use `issubclass()` with `Not`, nor does it make sense to create a subclass of `Not`, etc.

I'd prefer it if we could implement these as instances of `_SpecialForm` in the stubs, similar to what typeshed does for `Union` and `Optional`: https://github.com/python/typeshed/blob/33d1b169c1780529e8c2b91858caf58852d73220/stdlib/typing.pyi#L205-L219. This has the added advantage that I think the implementation of these special forms will become a little more similar to the implementations we've already added in `infer.rs` for `Union` and `Optional`

---

_Review comment by @AlexWaygood on `crates/red_knot_vendored/vendor/typeshed/stdlib/red_knot.pyi`:11 on 2025-01-03 11:20_

I think this variable is unused! The PEP-695 type parameters you're using in the class and function definitions below all create their own local-to-the-scope variables

---

_@AlexWaygood reviewed on 2025-01-03 11:21_

This is really nice. Considering how much more readable this makes our tests, I'm sold that this is the way to go. As such, my review here is mostly a review/commentary of your implementation rather than of the concept itself :-)

---

_@sharkdp reviewed on 2025-01-03 11:27_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:67 on 2025-01-03 11:27_

I like it! That's also the name used in C++, which has the same distinction between `assert` and `static_assert`.

---

_@MichaReiser reviewed on 2025-01-03 12:28_

More of a question than a comment: What are your thoughts on whether we should gate this behind a feature or make it testing only?

---

_@sharkdp reviewed on 2025-01-03 12:28_

---

_Review comment by @sharkdp on `crates/red_knot_vendored/vendor/typeshed/stdlib/red_knot.pyi`:5 on 2025-01-03 12:28_

Great suggestion — thank you. The implementation is also a bit cleaner now.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:110 on 2025-01-03 12:43_

minor: this might not be the best example, as I _believe_ `static_assert(int == int)` would also pass (or it _should_ do anyway, since it will evaluate to `True` at runtime, and we should be able to statically determine that). But the `is_equivalent_to` relation is not (necessarily) the same as a simple runtime equality relation. A better demonstration here might be `static_assert(is_equivalent_to(type, type[object]))` or `static_assert(is_equivalent_to(Intersection[LiteralString, Literal["a"]], Literal["a"]))`

(There might be simpler examples -- my main point is just that something where the runtime equality relation doesn't hold true would be best)

---

_@sharkdp reviewed on 2025-01-03 12:45_

---

_Review comment by @sharkdp on `crates/red_knot_vendored/vendor/typeshed/stdlib/VERSIONS`:344 on 2025-01-03 12:45_

> The type information for the package is published as a separate stubs package, `types-mypy-extensions`, and the source code for that stubs package is found in the [`stubs/mypy-extensions` directory](https://github.com/python/typeshed/tree/main/stubs/mypy-extensions) in typeshed.

Oh, interesting.

> Unlike typeshed's standard-library stubs, the stubs for these third-party packages in typeshed are not generally vendored by mypy (though pyright _does_ vendor them), as they're published as standalone packages to PyPI that you can `(uv) pip install` if you want your type checker to benefit from the type information provided by these packages. However, mypy makes an exception for the `mypy_extensions` package

:upside_down_face: 

> Is this relevant for us? Eh, after typing this all out, I'm not sure 🤷

I think so! If there is precedent for having these stubs in typeshed, that seems like a valid (future) solution to me.

> We almost certainly don't want to contribute our stubs for this package to typeshed upstream just yet

Of course

> This would involve a bunch of special casing in both our module resolver (we'd need to understand the `stubs/` directory) and our typeshed-sync GitHub workflow.

Seems like something I could try. Should we wait for the next Red Knot meeting to decide on whether we want this changeset or not?

> So a solution where we just apply a patch to typeshed's `VERSIONS` file at typeshed-sync time might indeed be simplest for now...

To be clear, here you are talking about yet another (simpler) solution that wouldn't put `red_knot.pyi` in `stubs/knot_extensions`, but in `stdlib/`, and then apply a patch to `stdlib/VERSIONS`, in order not having to add special casing logic to the module resolver?

---

_Review comment by @AlexWaygood on `crates/red_knot_vendored/vendor/typeshed/stdlib/VERSIONS`:344 on 2025-01-03 12:48_

> To be clear, here you are talking about yet another (simpler) solution that wouldn't put `red_knot.pyi` in `stubs/knot_extensions`, but in `stdlib/`, and then apply a patch to `stdlib/VERSIONS`, in order not having to add special casing logic to the module resolver?

correct!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2875 on 2025-01-03 12:50_

nit: I'd probably add an `is_red_knot` method to the `KnownModule` enum and then do this:

```suggestion
            | Self::RedKnotTypeOf => module.is_red_knot(),
```

which would be similar to the first branch of this big `match`

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class_base.rs`:97 on 2025-01-03 12:51_

hmm, maybe we _should_ allow subclassing from `Unknown`, since we allow subclassing from `Any` (and that's allowed at runtime as well)? Happy for us to just add a TODO comment with this question for now, though

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4283 on 2025-01-03 12:55_

I think the `class` variable here is actually an `InstanceType` object:

```suggestion
                    Err(TypeApiPredicateError::StaticAssertionError(Type::Instance(instance_ty)))
                        if instance_ty.class.is_known(db, KnownClass::Bool) =>
```

---

_Comment by @sharkdp on 2025-01-03 13:18_

> More of a question than a comment: What are your thoughts on whether we should gate this behind a feature or make it testing only?

Seeing that mypy and pyre have similar public APIs, I could imagine (part of) this becoming a public API for red knot that isn't feature-gated? The `Intersection`/`Not` constructors and `static_assert` are potentially interesting for users which don't care too much about compatibility with other type checkers (cf https://github.com/python/typing/issues/213). Some less-interesting parts like `is_singleton` could also be moved to `knot_extensions.internal` maybe?

Do we need to decide this right away? If not, can this stay public for now, or should we approach it very carefully and hide everything (by default)?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_api.rs`:12 on 2025-01-03 13:28_

A decent amount of logic in this file is devoted to call-signature checking for this API, and Carl has a draft PR up for a generalized solution to call-signature checking (https://github.com/astral-sh/ruff/pull/15200). I think currently you would _not_ benefit from the generalized solution there, because you have an early return on line 3000 in `infer.rs`, and Carl's solution is all implemented as part of the enum returned by `Type::call()`.

I do feel like ideally we wouldn't special-case these functions when it comes to emitting diagnostics for an incorrect number of arguments, we'd just use the generalized logic. But I'm also okay with adding this for now, since it helps us ensure that we use the APIs correctly while we're in a state where we don't yet have generalised call-signature checking.

---

_@AlexWaygood reviewed on 2025-01-03 13:28_

---

_Comment by @MichaReiser on 2025-01-03 13:30_

> Do we need to decide this right away? If not, can this stay public for now, or should we approach it very carefully and hide everything (by default)?

I'm generally leaning toward keeping the API intentionally limited and only expose features that we want to support long term (and consider part of the public API) because users will using them and it's very easy that we forget to revisit this decision before the release (unless we note it down somewhere?)

---

_@sharkdp reviewed on 2025-01-03 14:18_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:110 on 2025-01-03 14:18_

Oh, absolutely. I added the `is_equivalent_to(type, type[object])` example as well as `is_equivalent_to(tuple[int, Never], Never)`. I also added some other examples such as `is_equivalent_to(int | str, Union[int, str])` even if they evaluate to `True` at runtime.

---

_@sharkdp reviewed on 2025-01-03 14:28_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/type_api.rs`:12 on 2025-01-03 14:28_

> I do feel like ideally we wouldn't special-case these functions when it comes to emitting diagnostics for an incorrect number of arguments, we'd just use the generalized logic.

Yes, I was expecting that some of this would be superfluous once we have call signature checking. I think some of this is still required for the special forms, but I'd be glad to have this more generally be solved by call signature checking for predicates. I'll note it down as a future TODO, which I'll add to this PR in case #15200 will be merged first.

---

_Renamed from "[red-knot] Red Knot Type API" to "[red-knot] `knot_extensions` Python API" by @sharkdp on 2025-01-03 14:36_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_api.rs`:91 on 2025-01-03 14:51_

What if we had an inner enum inside a `KnownFunction` variant, similar to our existing solution for describing the subset of known functions that are constraint functions?

https://github.com/astral-sh/ruff/blob/0837cdd9314cb9ee1df087142af975d492e3e7ba/crates/red_knot_python_semantic/src/types.rs#L3064-L3086

I think that might be more extensible in the long run, since (as you've already identified), `assert_type()` is a predicate function very similar to the ones in `knot_extensions`, but it comes from a different module to these ones (`typing`/`typing_extensions`).

Using the `KnownFunction` enum for identifying these predicate functions would also have the advantage that it's less "stringly typed" here, and it would help maintain the property that the `KnownFunction` enum is an exhaustive enumeration of all functions that are special-cased in some way by red-knot.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_api.rs`:8 on 2025-01-03 14:55_

```suggestion
// TODO: redundant once more generalized call-signature checking is implemented?
#[derive(Debug)]
```

---

_@AlexWaygood approved on 2025-01-03 14:56_

LGTM, thank you!!

A couple more minor comments below, and we need to sort out the open question of where the `knot_extensions` stub will live (or it will get deleted in the next automated typeshed-sync PR). But other than that, this now looks great to me.

---

_@sharkdp reviewed on 2025-01-03 15:02_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/type_api.rs`:8 on 2025-01-03 15:02_

Note that this error is also returned when passing a wrong number of arguments to special forms like `Not` and `TypeOf`, which will not be handled by call signature checking.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_api.rs`:8 on 2025-01-03 15:14_

Oh, I missed that. Hmm, I think I'd probably prefer to emit those errors in the same `match` as we emit the equivalent errors for all our other special forms here: https://github.com/astral-sh/ruff/blob/706d87f239cb3af6562581d221bf6cd1f1636b66/crates/red_knot_python_semantic/src/types/infer.rs#L4963-L4980


---

_@AlexWaygood reviewed on 2025-01-03 15:14_

---

_@sharkdp reviewed on 2025-01-03 15:15_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/type_api.rs`:91 on 2025-01-03 15:15_

Done — thanks!

---

_@sharkdp reviewed on 2025-01-03 15:41_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/type_api.rs`:8 on 2025-01-03 15:41_

My initial idea (and my initial implementation) was trying to be very defensive in the sense that I tried to move the type API "out of the way", with most of the code in a separate module and only very few places where it interacted with existing type inference code. With all of your (great) suggestions, we have now moved to a version that has a tighter coupling to the existing code. This is fine for me, but it might be rather annoying to feature-gate all of this (see @MichaReiser's comment).

I can write separate argument handling logic for `TypeOf`/`Not` similar to what we have for `Annotated`, or try to unite that somehow, but it will make the coupling even stronger (and maybe lead to more code overall?). Does it make sense to wait with this until with have decisions on some higher-level questions (do we want to keep the currently proposed API? do we want to make it public or will all of the code be feature-gated? …)?

---

_@AlexWaygood reviewed on 2025-01-03 15:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4242 on 2025-01-03 15:54_

Oh, when I suggested using `KnownFunction`, I was imagining we'd set the `known` field on the `FunctionType` at inference time, like we do for our other `KnownFunction`s -- something like this (which is admittedly more verbose than what you currently have, but means that the `known()` and `is_known()` methods on `FunctionType` will continue to work as expected for all `KnownFunction` variants):

<details>

```diff
diff --git a/crates/red_knot_python_semantic/src/semantic_index/definition.rs b/crates/red_knot_python_semantic/src/semantic_index/definition.rs
index fc75d252d..d8d60d048 100644
--- a/crates/red_knot_python_semantic/src/semantic_index/definition.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/definition.rs
@@ -74,6 +74,11 @@ impl<'db> Definition<'db> {
             Some(KnownModule::Typing | KnownModule::TypingExtensions)
         )
     }
+
+    pub(crate) fn is_knot_extensions_definition(self, db: &'db dyn Db) -> bool {
+        file_to_module(db, self.file(db))
+            .is_some_and(|module| module.is_known(KnownModule::KnotExtensions))
+    }
 }
 
 #[derive(Copy, Clone, Debug)]
diff --git a/crates/red_knot_python_semantic/src/types.rs b/crates/red_knot_python_semantic/src/types.rs
index 64103b7a1..e42b2fa52 100644
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -3137,6 +3137,17 @@ impl KnownFunction {
         }
     }
 
+    pub fn into_type_api_function(self) -> Option<TypeApiFunction> {
+        match self {
+            Self::TypeApiFunction(function) => Some(function),
+            Self::RevealType
+            | Self::Len
+            | Self::Final
+            | Self::NoTypeCheck
+            | Self::ConstraintFunction(_) => None,
+        }
+    }
+
     fn try_from_definition_and_name<'db>(
         db: &'db dyn Db,
         definition: Definition<'db>,
@@ -3155,6 +3166,31 @@ impl KnownFunction {
             "no_type_check" if definition.is_typing_definition(db) => {
                 Some(KnownFunction::NoTypeCheck)
             }
+            "static_assert" if definition.is_knot_extensions_definition(db) => Some(
+                KnownFunction::TypeApiFunction(TypeApiFunction::StaticAssert),
+            ),
+            "is_subtype_of" if definition.is_knot_extensions_definition(db) => {
+                Some(KnownFunction::TypeApiFunction(TypeApiFunction::IsSubtypeOf))
+            }
+            "is_disjoint_from" if definition.is_knot_extensions_definition(db) => {
+                Some(KnownFunction::TypeApiFunction(TypeApiFunction::IsDisjointFrom))
+            }
+            "is_equivalent_to" if definition.is_knot_extensions_definition(db) => Some(
+                KnownFunction::TypeApiFunction(TypeApiFunction::IsEquivalentTo),
+            ),
+            "is_assignable_to" if definition.is_knot_extensions_definition(db) => Some(
+                KnownFunction::TypeApiFunction(TypeApiFunction::IsAssignableTo),
+            ),
+            "is_fully_static" if definition.is_knot_extensions_definition(db) => Some(
+                KnownFunction::TypeApiFunction(TypeApiFunction::IsFullyStatic),
+            ),
+            "is_singleton" if definition.is_knot_extensions_definition(db) => {
+                Some(KnownFunction::TypeApiFunction(TypeApiFunction::IsSingleton))
+            }
+            "is_single_valued" if definition.is_knot_extensions_definition(db) => Some(
+                KnownFunction::TypeApiFunction(TypeApiFunction::IsSingleValued),
+            ),
+
             _ => None,
         }
     }
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index b92722c36..cdea434c0 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -69,12 +69,12 @@ use crate::types::{
     typing_extensions_symbol, Boundness, CallDunderResult, Class, ClassLiteralType, FunctionType,
     InstanceType, IntersectionBuilder, IntersectionType, IterationOutcome, KnownClass,
     KnownFunction, KnownInstanceType, MetaclassCandidate, MetaclassErrorKind, SliceLiteralType,
-    Symbol, Truthiness, TupleType, Type, TypeAliasType, TypeApiFunction, TypeArrayDisplay,
+    Symbol, Truthiness, TupleType, Type, TypeAliasType, TypeArrayDisplay,
     TypeVarBoundOrConstraints, TypeVarInstance, UnionBuilder, UnionType,
 };
 use crate::unpack::Unpack;
 use crate::util::subscript::{PyIndex, PySlice};
-use crate::{Db, KnownModule};
+use crate::Db;
 
 use super::context::{InNoTypeCheck, InferContext, WithDiagnostics};
 use super::diagnostic::{
@@ -4234,22 +4234,21 @@ impl<'db> TypeInferenceBuilder<'db> {
     ) -> Option<Type<'db>> {
         let db = self.db();
 
-        match function.into_function_literal() {
-            Some(function)
-                if file_to_module(db, function.body_scope(db).file(db))
-                    .is_some_and(|module| module.is_known(KnownModule::KnotExtensions)) =>
-            {
-                let function = TypeApiFunction::try_from(function.name(db).as_str()).ok()?;
-
+        match function
+            .into_function_literal()
+            .and_then(|function| function.known(db))
+            .and_then(KnownFunction::into_type_api_function)
+        {
+            Some(api_function) => {
                 let argument_types = arguments.args.iter().map(|arg| {
-                    if function == TypeApiFunction::StaticAssert {
+                    if api_function.is_static_assert() {
                         self.infer_expression(arg)
                     } else {
                         self.infer_type_expression(arg)
                     }
                 });
 
-                let result = type_api::resolve_predicate(db, function, argument_types);
+                let result = type_api::resolve_predicate(db, api_function, argument_types);
 
                 match result {
                     Ok(ty) => Some(ty),
diff --git a/crates/red_knot_python_semantic/src/types/type_api.rs b/crates/red_knot_python_semantic/src/types/type_api.rs
index f1d5f945e..72360d8fb 100644
--- a/crates/red_knot_python_semantic/src/types/type_api.rs
+++ b/crates/red_knot_python_semantic/src/types/type_api.rs
@@ -81,21 +81,9 @@ pub enum TypeApiFunction {
     IsSingleValued,
 }
 
-impl TryFrom<&str> for TypeApiFunction {
-    type Error = ();
-
-    fn try_from(value: &str) -> Result<Self, Self::Error> {
-        match value {
-            "static_assert" => Ok(Self::StaticAssert),
-            "is_equivalent_to" => Ok(Self::IsEquivalentTo),
-            "is_subtype_of" => Ok(Self::IsSubtypeOf),
-            "is_assignable_to" => Ok(Self::IsAssignableTo),
-            "is_disjoint_from" => Ok(Self::IsDisjointFrom),
-            "is_fully_static" => Ok(Self::IsFullyStatic),
-            "is_singleton" => Ok(Self::IsSingleton),
-            "is_single_valued" => Ok(Self::IsSingleValued),
-            _ => Err(()),
-        }
+impl TypeApiFunction {
+    pub const fn is_static_assert(self) -> bool {
+        matches!(self, TypeApiFunction::StaticAssert)
     }
 }
```

</details>

But this would make the problem you pointed out elsewhere even worse -- we'd be integrating the handling of these functions into red-knot in a way such that they'd be harder to feature-gate.

I'm not sure feature-gating is a realistic possibility though... if we want to stop external users from making use of these APIs for now (which I agree is a reasonable idea), I think we might want to do that using a runtime check somehow rather than a compile-time check. While it would be _possible_ to try to squirrel away all the logic in a separate module, I think in the long run it'll be really confusing to have the logic for some special forms in a different place to the logic for all our other special forms. (And same for type-predicate functions -- I don't really want the logic for `is_subtype_of` to live in a completely different submodule to the logic for `assert_type`, since they're so similar conceptually!)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:32 on 2025-01-06 18:48_

Use `static_assert` here?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:32 on 2025-01-06 18:50_

Nit that may not be worth all the changes it would imply in this PR: I weakly prefer using annotated function arguments over non-local name references, because the semantics of the former are very clear, whereas our handling of non-local name references (particularly declared but unbound ones, as used here) could change in future. For example, I can easily imagine that in future all of these references might emit an unbound-name diagnostic.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:244 on 2025-01-06 19:00_

Interesting. Given that this is Python code, I could see an argument for using Python semantics here (silently accepting anything whose `__bool__` evaluates to `Literal[True]`) rather than the stricter Rust semantics (requiring explicit conversion to bool). (In other words, not emitting any error here.)

Based on your usage of this API so far, do you feel that this would be too error-prone?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:320 on 2025-01-06 19:47_

Why is `TypeOf` a bracketed special form, rather than a function `type_of`?

If it were a function, it seems like once we have generics it wouldn't even require special-casing, it would just be a function with signature `def type_of[T](obj: T) -> T: ...`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/type_api.rs`:8 on 2025-01-06 19:58_

I would prefer to not maintain different ways of doing the same thing in different places, depending on whether they are "red knot type API" vs "standardized type API" -- for example, I think `reveal_type` and `assert_type` should be implemented in the same way as red-knot type API functions. (Currently this PR implements red-knot type API functions differently from `reveal_type`.) The distinction of whether some API is standardized vs red-knot-specific is incidental and subject to change (I could easily see `static_assert` becoming standardized in the future, for example), and I would rather minimize the extent to which such differences are hardcoded in Rust.

I also don't think that the question of what we expose publicly needs to be closely tied to the question of code-level integration. I think whatever feature gating we do should probably not happen at the Rust implementation level (that is, trying to turn off or on certain Rust code paths in type inference), but rather at the Python level; that is, in the type stub for the `knot_extensions` module. This could mean only conditionally including the type stub for `knot_extensions` at all, or having some conditional definitions of things in the type stub. If you can't import the known-function and known-instance types that make up the type API, then it doesn't matter whether the code paths exist for those; you won't be able to enter those code paths anyway.

So I am generally in favor of making the code in this PR more integrated with the rest of red-knot, not less. That includes using our general support for call-checking (once it lands) to implement checking arguments to type-API functions, and implementing bracketed-special-form handling for type API special forms in the same place we do so for other bracketed special forms.

---

_Comment by @carljm on 2025-01-06 20:04_

> I'm generally leaning toward keeping the API intentionally limited and only expose features that we want to support long term (and consider part of the public API)

For what it's worth, there's nothing in the API added in this PR that I have reservations about supporting long-term as public API, in principle. Of course it's always possible that we realize we made mistakes in details of how we defined or named the API, and we want to change these things in future and potentially have to take backward-compatibility into account with those changes. But this is inevitable.

---

_Review comment by @carljm on `crates/red_knot_vendored/vendor/typeshed/stdlib/VERSIONS`:344 on 2025-01-06 20:09_

Given that we will need some kind of typeshed-patching for now, I think we should do whatever form of it is easiest to implement.

At some point we'll definitely need to add support to our module resolver for non-stdlib stubs packages, but it's not clear yet if we'll specifically need support for vendoring `typeshed/stubs`, and I don't think this PR needs to be the driver for adding that support.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2602 on 2025-01-06 20:11_

I have mixed feelings even about using the `KnotExtensions` prefix here; we don't do that for special forms from other modules, unless it's necessary for other reasons (e.g. `TypingSelf`, due to Rust keyword `Self`), and I can easily see any of these becoming not red-knot-specific in future. But I don't feel strongly, it's easy to remove this prefix in future.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3125 on 2025-01-06 20:12_

Similarly, I wonder why we need `TypeApiFunction` nested enum here, rather than including the type-api functions as top-level known functions in `KnownFunction` enum?

---

_@AlexWaygood reviewed on 2025-01-06 20:14_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_api.rs`:8 on 2025-01-06 20:14_

(More for @carljm's benefit: I responded to this a little bit at https://github.com/astral-sh/ruff/pull/15103#discussion_r1901926256)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3001 on 2025-01-06 20:14_

As mentioned in another comment, I would prefer not to have special handling for "red knot type api" functions; I think they should work in the same way as `reveal_type` and `assert_type` (which are not red-knot specific).

Perhaps we don't like the way `reveal_type` is currently handled in `Type::call` and `CallOutcome`, and want to adjust it. But I don't think there should be different paths for it vs red-knot type API.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4218 on 2025-01-06 20:24_

I think this should just use the existing `INVALID_TYPE_FORM` diagnostic rule.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4196 on 2025-01-06 20:29_

Nit: I would expect this method to be located down near the other type-expression inference methods (that is, near where it's used.)

And similar to other comments, I would rather see unified/integrated slice/arity handling for all of our special forms, rather than special handling just for the red-knot-specific ones. They are all just bracketed special forms; whether they are red-knot specific or not should not be fundamental to their implementation.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4266 on 2025-01-06 20:29_

I would expect this to emit the same diagnostic as any other wrong-arity call, not a special diagnostic specific to red-knot type-api.

---

_Review comment by @carljm on `crates/red_knot_vendored/vendor/typeshed/stdlib/knot_extensions.pyi`:21 on 2025-01-06 20:33_

What's the advantage of using these dedicated types vs just having them all return `bool` in the stub? Either way the stub-annotated return type isn't used as-is, we have to special-case it. But this way it seems like the stub is just lying about the actual inferred return type.

---

_@carljm reviewed on 2025-01-06 20:33_

This is really excellent, thank you! I look forward to using it, and I think red-knot users will also find it valuable.

---

_@AlexWaygood reviewed on 2025-01-06 20:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:320 on 2025-01-06 20:39_

> it would just be a function with signature `def type_of[T](obj: T) -> T: ...`?

That would imply that if the function were passed an object of type `int`, it would return an object of type `int`, but that's not what the function does at all (it returns the type `int` itself, not an object of type `int`). I think we would need the TypeForm proposal to annotate such a function in such a way that it would require no red-knot special-casing?

---

_@AlexWaygood reviewed on 2025-01-06 20:40_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:320 on 2025-01-06 20:40_

However, I agree that a special-cased function feels like it might be a nicer API here than a bracketed special form!

---

_@sharkdp reviewed on 2025-01-06 21:28_

---

_Review comment by @sharkdp on `crates/red_knot_vendored/vendor/typeshed/stdlib/knot_extensions.pyi`:21 on 2025-01-06 21:28_

I was expecting this question to come up :smile:. Honestly, I didn't really know what to put here. Yes, `bool` is the union of both possible return types that we would could infer (either `Literal[True]` or `Literal[False]`). But it wouldn't ever be useful for a type checker to fall back to this `bool` return type annotation, since no static conclusions can be drawn from it, right? Or is that exactly why you want it to be annotated as `bool`? For other type checkers that do not special case these functions?

I think I wanted to express that these are functions on types, not on objects. Using `_Xyz[S, T]` as a return type was my way of saying: the return type depends on the argument types. But of course I couldn't express that the return type does not *also* depend on those "phantom data" objects `x`/`y`.

This is also why I implemented these predicates as generic classes like `IsEquivalentTo[S, T]` initially, as that seemed to imply to me that a new type (`IsEquivalentTo[S, T]`) is being computed from two other types (`S` and `T`). But I understand that that's not great either.

So I'm happy to change it to `bool`. And I can also add some documentation for readers of that stub file.

---

_@carljm reviewed on 2025-01-06 21:32_

---

_Review comment by @carljm on `crates/red_knot_vendored/vendor/typeshed/stdlib/knot_extensions.pyi`:21 on 2025-01-06 21:32_

Yes, it makes sense how you got here :) But my feeling is that neither return type is directly useful, and `bool` is both accurate and simple.

> I wanted to express that these are functions on types

Yeah; I think the way to do this is would be via annotating the arguments as `TypeForm`, once PEP 747 is accepted.

---

_@sharkdp reviewed on 2025-01-06 21:46_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:244 on 2025-01-06 21:46_

No, that makes sense to me, I hadn't thought about that.

> Based on your usage of this API so far, do you feel that this would be too error-prone?

I don't think so.

---

_@sharkdp reviewed on 2025-01-06 21:53_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:320 on 2025-01-06 21:53_

> Why is `TypeOf` a bracketed special form, rather than a function `type_of`?

Because I wanted it to be usable in a type annotation, and it seems like a function call as in `x: type_of(str) = …` would not be valid syntax according to the [typing spec](https://typing.readthedocs.io/en/latest/spec/annotations.html)?

---

_@carljm reviewed on 2025-01-06 21:55_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:320 on 2025-01-06 21:55_

Oh, interesting! That's a good reason. Perhaps we should include a test of using it in an annotation here?

---

_@sharkdp reviewed on 2025-01-06 21:58_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:320 on 2025-01-06 21:58_

`TypeOf[str]` *would* be valid syntax (similar to `Union` etc). But that doesn't eradicate all syntax problems with `TypeOf`. Because I might want to write `TypeOf[2 + 3]`, but that's not valid according to:
```
                           | <type> '[' <Any> ']'
                           | <type> '[' name ']'
                                 (where name must refer to a valid in-scope class
                                  or TypeVar)
```

This is also why I wrote in the PR description that I was thinking about replacing `TypeOf[…]` with `ClassLiteral[…]`, which could only be used for naming class literal types and nothing else.

It seems like `TypeOf[…]` could be more generally useful, but I haven't actually come up with any other examples yet.

---

_@sharkdp reviewed on 2025-01-06 22:00_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3125 on 2025-01-06 22:00_

It allows me to exhaustively match on all type API functions later without having to use `unreachable!`. But if we're all okay with a tighter integration of this code here, I can see if it makes sense to flatten this.

---

_@AlexWaygood reviewed on 2025-01-06 22:06_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:320 on 2025-01-06 22:06_

Maybe something like this would follow the letter (though possibly not the spirit 😅) of the spec:

```py
type X = type_of(2 + 3)
spam: X
```

---

_@carljm reviewed on 2025-01-07 00:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3125 on 2025-01-07 00:47_

> It allows me to exhaustively match on all type API functions later

Right. I think the idea of flattening is predicated on the idea that we _shouldn't_ have any place where we match on "all type API functions" (as opposed to "all known functions with special-cased call handling").

---

_@carljm reviewed on 2025-01-07 00:53_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:320 on 2025-01-07 00:53_

I don't think we need to worry about making `TypeOf` usable in an annotation with arbitrary non-typeform expressions. You can always just assign the expression to a variable `x` and use `TypeOf[x]` instead.

I will say that `TypeOf` is the API in this PR whose use cases are least clear to me. If we really only have use cases for expressing class literal types in an annotation, it would probably be trivial to allow `Literal[A]` for that (the same way we currently output those types). Though we may want to find a different syntax to output those types, in which case we'd want to add matching red-knot API for spelling them, too.

> Maybe something like this would follow the letter (though possibly not the spirit 😅) of the spec:
> 
> ```python
> type X = type_of(2 + 3)
> spam: X
> ```

I don't think this follows the letter or spirit of [the spec](https://typing.readthedocs.io/en/latest/spec/aliases.html#type-statement) :)

> As with typing.TypeAlias, type checkers should restrict the right-hand expression to expression forms that are allowed within type annotations.

But there's no need to use a `type` statement for this at all, the simpler way would be:

```py
x = 2 + 3
spam: TypeOf[x]
```

---

_@sharkdp reviewed on 2025-01-07 13:22_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:320 on 2025-01-07 13:22_

Ok, I understand I can leave it as is for now. I added a test which uses `TypeOf` in annotation position and demonstrates the difference between `TypeOf[str]` and `type[str]`:

```py
class Base: ...
class Derived(Base): ...

# `TypeOf` can be used in annotations:
def type_of_annotation() -> None:
    t1: TypeOf[Base] = Base
    t2: TypeOf[Base] = Derived  # error: [invalid-assignment]

    # Note how this is different from `type[…]` which includes subclasses:
    s1: type[Base] = Base
    s2: type[Base] = Derived  # no error here
```

---

_@sharkdp reviewed on 2025-01-07 20:11_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/type_api.rs`:8 on 2025-01-07 20:11_

> So I am generally in favor of making the code in this PR more integrated with the rest of red-knot, not less. That includes using our general support for call-checking (once it lands) to implement checking arguments to type-API functions, and implementing bracketed-special-form handling for type API special forms in the same place we do so for other bracketed special forms.

This is all resolved now.

---

_@sharkdp reviewed on 2025-01-07 20:12_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3125 on 2025-01-07 20:12_

Ok, the type API functions are now handled in the same places like `RevealType` etc.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:4266 on 2025-01-07 20:13_

Ok, rebased my branch on top of your call-signature checking branch, so this now uses your implementation.

---

_@sharkdp reviewed on 2025-01-07 20:13_

---

_@sharkdp reviewed on 2025-01-07 20:27_

---

_Review comment by @sharkdp on `crates/red_knot_vendored/vendor/typeshed/stdlib/knot_extensions.pyi`:21 on 2025-01-07 20:27_

So I'm just going to mention this one last time and then I'll be quiet :smile:. The current version of the API does feel good to me and I think it's more or less intuitive. I'm personally fine with merging it as is.

But upon closer inspection, there are some things that are "special".

The type predicates like `is_fully_static` etc. do not work like usual Python functions: For all other functions, we call `infer_expression` on the arguments in a function call. But for these predicates, we call `infer_type_expression`.

Similarly, the special form `TypeOf` does not work like usual special forms (although `Annotated` can also contain an non-type-expression in the second argument). For all other special forms, we call `infer_type_expression` for the bracketed arguments to the special form. For `TypeOf`, we call `infer_expression`.

For `TypeOf`, we already discussed that it might be necessary to leave it as a special form, as a call expression can not occur in type annotations.

But the functions still feel a bit strange to me, and also require some special-casing in the implementation. It seems to me that even if we had PEP 747 and annotated the parameter types of these functions as `TypeForm`s, we should *still* call `infer_expression` instead of `infer_type_expression` for calls to such functions (I haven't read the full PEP, but [the `split_union` example here](https://peps.python.org/pep-0747/#introspecting-type-form-objects) seems to imply that `typ` internally is of type `UnionType`, for example — not an actual union type).

Again, I'm personally fine with this special-casing, but I thought it would be worth calling out one last time. The alternative would be to make these predicates special forms that could be called via `IsFullyStatic[int]` or maybe these would be generic classes with a `__bool__` method that would return `Literal[True]`/`Literal[False]` accordingly, such that we could write `static_assert(IsFullyStatic[int]())`. Or potentially `static_assert(IsFullyStatic[int])` if we implement it on the meta-class(?).

(the `Intersection` and `Not` type forms, and the `static_assert` function act normal)

---

_@sharkdp reviewed on 2025-01-07 20:48_

---

_Review comment by @sharkdp on `crates/red_knot_vendored/vendor/typeshed/stdlib/knot_extensions.pyi`:24 on 2025-01-07 20:48_

I annotated these using `Any`, because now with call signature checking, I couldn't call `is_fully_static(type[Any])` when they were annotated using `object` because apparently `type[Any]` (the type, not the expression) is not assignable to `object`. And using generics is not an option yet.

---

_@carljm reviewed on 2025-01-07 20:52_

---

_Review comment by @carljm on `crates/red_knot_vendored/vendor/typeshed/stdlib/knot_extensions.pyi`:24 on 2025-01-07 20:52_

Everything should be assignable to `object`. If `type[Any]` is not assignable to `object`, that's a separate bug that we should fix.

---

_@AlexWaygood reviewed on 2025-01-07 20:55_

---

_Review comment by @AlexWaygood on `crates/red_knot_vendored/vendor/typeshed/stdlib/knot_extensions.pyi`:24 on 2025-01-07 20:55_

(Though separately, `Any` is arguably a better annotation for these parameters anyway, since `object` implies "it would be valid to pass literally any object to these functions", and that's not really true — in fact, the object passed into several of these has to be a Python object representing a type (a typeform) of some kind)

---

_@AlexWaygood reviewed on 2025-01-07 20:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_vendored/vendor/typeshed/stdlib/knot_extensions.pyi`:24 on 2025-01-07 20:57_

Our policy at typeshed is to prefer `Any` for situations like this where the "true type" is inexpressible, anyway

---

_Review comment by @carljm on `crates/red_knot_vendored/vendor/typeshed/stdlib/knot_extensions.pyi`:24 on 2025-01-07 21:22_

If you rebase on top of https://github.com/astral-sh/ruff/pull/15332 you should be able to go back to `object` here.

---

_@carljm reviewed on 2025-01-07 21:22_

---

_@carljm reviewed on 2025-01-07 21:27_

---

_Review comment by @carljm on `crates/red_knot_vendored/vendor/typeshed/stdlib/knot_extensions.pyi`:24 on 2025-01-07 21:27_

Oh, Alex's comments didn't live-update for me. I can see the rationale for preferring `Any` here, pending `TypeForm` availability. In that case, feel free to leave it, and not bother rebasing on my PR :)

---

_@carljm reviewed on 2025-01-07 21:50_

---

_Review comment by @carljm on `crates/red_knot_vendored/vendor/typeshed/stdlib/knot_extensions.pyi`:21 on 2025-01-07 21:50_

Your functions aren't the first example of this: the existing `typing.cast` function (which we haven't yet implemented support for, but will need to) also requires its first argument to be interpreted as a type expression rather than a value expression.

PEP 747 generalizes this; it requires that an expression whose "type context" is a `TypeForm` type (e.g. the RHS of an annotated assignment where the annotation is a `TypeForm` type, or an argument matched to a parameter annotated as a `TypeForm` type) be interpreted as a type expression rather than a value expression. If the expression is a valid type expression, it is assignable to `TypeForm`; if it is a valid type expression spelling a type assignable to `T`, then it is assignable to `TypeForm[T]`.

(The `split_union` example in the PEP is not a counter-example to this. A `TypeForm` type is indeed never an "actual union type"; `TypeForm[T | S]` represents the set of runtime objects that can result from a valid type expression for the type `T | S`. The type `types.UnionType` overlaps with `TypeForm[T | S]`; the type `T | S` is disjoint from the type `TypeForm[T | S]`.)

This kind of contextual type evaluation is something we don't support yet, but will need to. It comes up in other cases besides `TypeForm` as well; Eric Traut has some good examples at https://discuss.python.org/t/pep-747-typeexpr-type-hint-for-a-type-expression/55984/67

---

_@carljm reviewed on 2025-01-07 21:55_

---

_Review comment by @carljm on `crates/red_knot_vendored/vendor/typeshed/stdlib/knot_extensions.pyi`:21 on 2025-01-07 21:55_

The call-checking support will require changes to handle contextual type evaluation; we'll need to match arguments to parameters before inferring argument types. I felt that it still made sense to do the simpler implementation now, and adjust it when we add contextual inference support.

---

_@AlexWaygood reviewed on 2025-01-07 21:59_

---

_Review comment by @AlexWaygood on `crates/red_knot_vendored/vendor/typeshed/stdlib/knot_extensions.pyi`:21 on 2025-01-07 21:59_

> Your functions aren't the first example of this: the existing `typing.cast` function (which we haven't yet implemented support for, but will need to) also requires its first argument to be interpreted as a type expression rather than a value expression.

similar for `assert_type` as well

---

_@sharkdp reviewed on 2025-01-07 22:12_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1795 on 2025-01-07 22:12_

We currently do not emit a (proper) diagnostic for calls like `reveal_type()` or `static_assert()` (this is a pre-existing issue, pre-dating call-signature checking). I'll look into that tomorrow.

---

_@sharkdp reviewed on 2025-01-07 22:16_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1800 on 2025-01-07 22:16_

This is not quite consistent with what the single-argument functions do. We probably shouldn't use `Type::Unknown` for the return type, but rather for the `ty_a` and `ty_b`, in case the `[…, …]` pattern doesn't match. I don't fully understand yet when that happens (i.e.: when would `binding.parameter_tys()` ever return something that doesn't match the expected signature, or when would `.first_parameter()` return `None` for a single-argument function?). Maybe only when there is an inconsistency between stubs and the expected number of arguments here?

I'll look into that tomorrow.

---

_@carljm reviewed on 2025-01-07 22:28_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1800 on 2025-01-07 22:28_

`binding.parameter_tys()` is a slice of `Type<'db>` (not `Option<Type<'db>>`), of length equal to the number of parameters of the function (regardless of the number of arguments provided in the call). `binding.first_parameter()` (which should probably be `binding.first_parameter_ty()` instead, or we shouldn't even have a special-case method at all for the first parameter) will only return `None` if the function being called doesn't take any parameters at all. If it has a first parameter, but the call didn't supply an argument for it, we'll get `Unknown`.

Currently `parameter_tys` always reflects the provided arguments, even if they aren't valid for the annotated parameter type (in which case you'll get an error, but still the actual - invalid - argument type in `parameter_tys`). It also doesn't reflect defaults; if the argument is not provided, you'll get Unknown, even if the parameter has a default. Neither of these are a design choice that I feel strongly about; I just didn't need anything different, and so I kept it simple. It would be easy to change either of these behaviors in `bind_call`, if that would make `parameter_tys` more useful in practice. I didn't get a lot of experience using `parameter_tys`, since `reveal_type` was the only use case I had.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:134 on 2025-01-07 23:26_

The use of "enforce" is a bit confusing to me here. It seems to me that `static_assert` can be used to _check_ red-knot's understanding of narrowing constraints, but I'm not sure in what sense it "enforces" them.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:145 on 2025-01-07 23:27_

Maybe we don't need to include the `utm_source` in this link 😆 
```suggestion
<https://docs.python.org/3/library/stdtypes.html#truth-value-testing>
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:148 on 2025-01-07 23:29_

Seems like it would be useful to give a couple explicit examples of statically-known-truthy cases as well (e.g. `static_assert(True)`, `static_assert(5)`...

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1785 on 2025-01-07 23:37_

Is this line necessary? Shouldn't the stub return type suffice here?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1800 on 2025-01-07 23:39_

In this case I think the fallback case is just unreachable (unless the stub for `is_equivalent_to` were changed to not have exactly two parameters).

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3257 on 2025-01-07 23:45_

In order to support `typing.cast` and `typing.assert_type`, I think we'll need a finer-grained version of this that allows some "type" and some "value" arguments of the same function. Doesn't have to happen in this PR, though.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:272 on 2025-01-07 23:48_

Is there value in separating this case from the one immediately below? I found this surprising when I saw it in the tests.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:308 on 2025-01-07 23:50_

Shouldn't this case be unreachable, because `truthiness` must be either always-false, ambiguous, or always-true, and we won't create a `CallOutcome::StaticAssertionError` if its always-true? I would be inclined to explicitly mark it as unreachable.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:684 on 2025-01-07 23:51_

```suggestion
    /// Makes sure that the argument of `static_assert` is statically known to be true.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:105 on 2025-01-07 23:52_

The db is already available as `context.db()`, we shouldn't need it as a separate argument.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:177 on 2025-01-07 23:52_

Same as above, the `InferContext` already gives us the db.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2011 on 2025-01-07 23:54_

Why do we need to add a `db` argument to a method of `TypeInferenceBuilder`? It should already be available as `self.db()`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2493 on 2025-01-07 23:55_

As mentioned elsewhere, we'll need per-argument granularity on this soon, but it doesn't have to be in this PR.

---

_@carljm reviewed on 2025-01-07 23:58_

---

_Comment by @carljm on 2025-01-07 23:59_

The integration/flattening looks great!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3257 on 2025-01-08 00:08_

```suggestion
    const fn takes_type_expression_arguments(self) -> bool {
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call.rs`:307 on 2025-01-08 00:09_

I think rustfmt may have given up on this file at this point because of all the macro usage 😆

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3226 on 2025-01-08 00:14_

usually I'm all for an exhaustive `match`, but here I think it's probably fine to simplify a little bit -- since all constraint functions are guaranteed to be wrapped in the first variant, there's probably nothing gained here by explicitly listing all the other variants

```suggestion
            _ => None,
```

---

_@AlexWaygood reviewed on 2025-01-08 00:15_

Very nice! A couple of nitpicks on top of Carl's comments. And it still needs an edit to https://github.com/astral-sh/ruff/blob/main/.github/workflows/sync_typeshed.yaml to make sure we don't lose the `knot_extensions` stub file in the next automated typeshed sync

---

_@sharkdp reviewed on 2025-01-08 05:23_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:145 on 2025-01-08 05:23_

Hahaha

Credit where credit is due!

For reference, I literally asked something like "where is ... specified in the Python documentation", because I couldn't find it otherwise. And it worked! Didn't notice it was sneaky and added a UTM parameter though 😀

---

_@sharkdp reviewed on 2025-01-08 07:02_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:148 on 2025-01-08 07:02_

I think I did that for all variants here except for `static_assert(True)` because that is already covered above. But we have:
```py
static_assert(1)
# …
static_assert((0,))
# …
static_assert("a")
# …
static_assert(b"a")
```

But I guess it doesn't hurt to add `static_assert(True)` here again.

---

_@sharkdp reviewed on 2025-01-08 07:04_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1800 on 2025-01-08 07:04_

Don't we need to be careful with `unreachable!(…)` because someone might supply a custom typeshed?

---

_@sharkdp reviewed on 2025-01-08 07:35_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:134 on 2025-01-08 07:35_

My idea was the following. I could see how a developer who makes use of a narrowed type in a particular (complicated) branch might want to add a static assertion like this at the top of the branch to get better error messages in case *something* changes that would result in a non-narrowed type.

This wouldn't necessarily be limited to changes in the type checker, but also to changes in the code. Take this example:
```py
def f(x: int):
    if x != 0:
        static_assert(x != 0)

        # make use of the fact that x is statically known to be nonzero.
```
If a fellow developer comes along and makes a seemingly "stylistic" change to `0 != x` (YODA style), that static assertion would fail. And that failure would potentially provide a better feedback to the developer making that change than whatever diagnostic we would emit in the body of that branch due to the `int & ~Literal[0]` -> `int` change.

So in this sense, the static assertion helps "enforce" that narrowing constraint. It's not so much used to test a type checkers understanding of narrowing, it's more like a guard that helps to make sure that (seemingly innocuous) changes to the code don't lead to a non-narrowed type in that branch.

That said, I'm not sure if someone would actually do this. And I might have a slightly wrong understanding of the word "enforce", so I'm happy to change this sentence to whatever we think is more appropriate. Or to remove this entire example.

---

_@sharkdp reviewed on 2025-01-08 07:40_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:134 on 2025-01-08 07:40_

> And that failure would potentially provide a better feedback to the developer

That actually makes me think: if we see `static_assert` as a useful tool for our users, should we support an optional second argument of type `LiteralString` for user-defined error messages? Like C++'s [`static_assert`](https://en.cppreference.com/w/cpp/language/static_assert)?

Edit: I went ahead and implemented this.

---

_@sharkdp reviewed on 2025-01-08 07:42_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1785 on 2025-01-08 07:42_

No, it's not. Thanks.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/call.rs`:307 on 2025-01-08 07:50_

Yes :frowning_face:. I formatted it manually.

---

_@sharkdp reviewed on 2025-01-08 07:50_

---

_@InSyncWithFoo reviewed on 2025-01-08 08:55_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/src/types.rs`:3257 on 2025-01-08 08:55_

I think this method will break even further when functions accepting `TypeForm`s are added to the mix:

```python
# User-defined; no hardcoding
def f(a: int, b: str, v: TypeForm[T]) -> list[T]: ...
```

Granted, `TypeForm` is not a thing yet, but it likely will, given how the type system has been growing.

---

_@sharkdp reviewed on 2025-01-08 09:04_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1800 on 2025-01-08 09:04_

> which should probably be `binding.first_parameter_ty()` instead, or we shouldn't even have a special-case method at all for the first parameter

I changed `first_parameter_ty` to `one_parameter_ty()` (with a change to the logic as well), and added `two_parameter_tys()`. These are intended as slightly more easy to use companions to `parameter_tys()` for functions that expect exactly one parameter or exactly two parameters.

---

_@sharkdp reviewed on 2025-01-08 09:11_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/call.rs`:272 on 2025-01-08 09:11_

I wanted a slightly easier to understand message for the (presumably most common) case where your assertion condition actually evaluates to `False`. Like in a comparison that's simply not true.

And I didn't want to re-use this message ("evaluates to `False`") for cases like `static_assert(<expression of type None>)` because it could be confusing, and because I wanted to add more context (show the actual type in the message), which seems not very helpful for cases where the type is `Literal[False]`.

I can see how that would be slightly confusing if you see those side-by-side in the tests, but do you think that any of these messages is actually confusing?

---

_@sharkdp reviewed on 2025-01-08 09:31_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/call.rs`:308 on 2025-01-08 09:31_

I added a new `StaticAssertionErrorKind` enum that describes the various cases. This allows us to remove this case without having to use `unreachable!`.

---

_@sharkdp reviewed on 2025-01-08 09:33_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:2493 on 2025-01-08 09:33_

Ok, I'll leave this as a follow-up task.

---

_@sharkdp reviewed on 2025-01-08 10:45_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3257 on 2025-01-08 10:45_

> I think this method will break even further

I'm not sure that's true? If we see a call like `f(1, "a", int | str)`, we will eventually need to check if `int | str` is a valid type expression. And for this purpose, we *may* want to call `infer_type_expression` on that `int | str` AST node. But *inside* the function `f`, the type of `v` will not be `int | str` (which is what `infer_type_expression` returns). It will be a TypeForm object instead.

---

_@sharkdp reviewed on 2025-01-08 10:52_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3257 on 2025-01-08 10:52_

I think this is not a Python typing question, it's an implementation detail. `assert_type(val, typ)` is a good example. In order to implement it, it makes sense to call `infer_expression` on `val`, and `infer_type_expression` on `typ`. But if I understand things correctly, this has nothing to do with whether or not `typ` is annotated as `Any` or as `TypeForm`.

And for your example function `f`, if we call `f(…, …, int | str)`, we would never infer `v` to be of type `int | str`, no matter how it is annotated.

---

_@sharkdp reviewed on 2025-01-08 10:52_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3257 on 2025-01-08 10:52_

https://pyright-play.net/?enableExperimentalFeatures=true&code=MQAgDgngTglg5gCwC4C4RQKZgPZSQWRgGciYA7OfbAEwFcAbDAZW1qgGMMBeAMwEN6RDAChhPKNgC2IJBDDk4AfQwAPJBjKlsmkDEk48IACpyMAMVyTR1DDxA8A2kYC6ACj5pySADQgARmhESFC%2BAG5oJmDmlk7OAJQgALQAfCD0xEixKMIguegYoRgCirJRrqFxonn5SGxkIA7OomKuAIy%2BAER8Hb5eIAA%2BIEFQcUA

---

_@sharkdp reviewed on 2025-01-08 11:27_

---

_Review comment by @sharkdp on `.github/workflows/sync_typeshed.yaml`:52 on 2025-01-08 11:27_

This could probably be something like
```suggestion
          (
            echo "# Patch applied for red_knot:"
            echo "knot_extensions: 3.0-"
          ) >> ruff/crates/red_knot_vendored/vendor/typeshed/stdlib/VERSIONS
```
but I kept it simple for now, as I don't know what's supported in the standard shell

---

_Merged by @sharkdp on 2025-01-08 11:52_

---

_Closed by @sharkdp on 2025-01-08 11:52_

---

_Branch deleted on 2025-01-08 11:52_

---

_Comment by @sharkdp on 2025-01-08 11:53_

Decided to merge this now to unblock work on #15194. Feel free to add post-merge comments and I will address them.

---

_@carljm reviewed on 2025-01-08 15:17_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:134 on 2025-01-08 15:17_

I understand now what you mean by "enforce", from the developer's perspective; this all seems fine to me.

---

_@carljm reviewed on 2025-01-08 15:21_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1800 on 2025-01-08 15:21_

> Don't we need to be careful with `unreachable!(…)` because someone might supply a custom typeshed?

Ah yes, good point!

---

_@carljm reviewed on 2025-01-08 15:29_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3257 on 2025-01-08 15:29_

> But if I understand things correctly, this has nothing to do with whether or not `typ` is annotated as `Any` or as `TypeForm`.

This doesn't quite make sense to me; I'm not sure I understand what you're saying in your comments here.

In the current type system, without `TypeForm`, certain functions have to be special-cased so that we know (some of) their arguments must be valid type expressions (and we know to infer them as type expressions, so we have the right types for special-casing the behavior of those functions, e.g. their return type, or some special diagnostics they might emit). These include specified functions such as `typing.cast` and `typing.assert_type`, as well as red-knot-specific functions introduced in this PR.

In this PR, we only handle the case that all of a function's parameters should be inferred as type expressions. For `typing.cast` and `typing.assert_type`, in the short term, we will have to expand this special-casing so we can mark only some parameters as type expressions.

In the longer term, assuming PEP 747 is accepted, `TypeForm` will generalize this, so we should be able to rely only on the type annotations of the function parameters to decide which arguments should be inferred as type expressions. If the parameter is annotated as a `TypeForm`, that will be sufficient for us to know that we need to infer any argument for that parameter as a type expression. Once `TypeForm` exists and is used in stubs, some functions that are currently special-cased will no longer require special-casing at all (e.g. `typing.cast` can be spelled as `def cast[T](typ: TypeForm[T], expr: object) -> T: ...` and needs no additional handling), while others (e.g. `is_subtype_of`) will still require special handling in the type checker to get the correct return type, but will no longer require explicit special-casing to decide which parameters to infer as type expressions.

So I do think this has something to do with whether `typ` is annotated as `Any` or `TypeForm`. I wouldn't phrase it as "`TypeForm` will break `takes_type_expression_arguments`", but rather as "`TypeForm`, plus contextual type inference of function parameters, will eventually make `takes_type_expression_arguments` unnecessary."

Am I missing your point?

---

_@carljm reviewed on 2025-01-08 15:32_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:272 on 2025-01-08 15:32_

Nope, I don't think either message is confusing on its own. And I see the rationale for splitting them; in particular, that the "actual type" context is very useful for the "is falsy" case, but is redundant/noisy for the "is `Literal[False]`" case.

---

_@sharkdp reviewed on 2025-01-08 19:44_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3257 on 2025-01-08 19:44_

Sorry for the confusion I caused here. I think it was mostly just miscommunication (by me), and we probably all agree. To summarize (thank you @carljm for the proper wording):

- For functions like `is_fully_static`/`cast`/`assert_type` but also `f` in @InSyncWithFoo's example, we will most definitely want to call `infer_type_expression` on the (relevant) arguments in order to ensure that they represent valid type expressions. For the example above, if we see `f(…, …, int | str)`, we will then get the answer `int | str` (the actual type) from calling `infer_type_expression` (instead of `typing.UnionType`, which is what we would get if we call `infer_expression).
- For (future) functions annotated with `TypeForm`, we will then wrap the result of that call (`int | str`) in `TypeForm` to get a new type, `TypeForm[int | str]`, which is what the inferred parameter type of `v` would ideally be for this call.
- As long as we don't have `TypeForm`, we need to hard-code the knowledge of whether to call `infer_expression` or `infer_type_expression` *somewhere*. Since we don't have `TypeForm` implemented yet, we currently set the parameter type for special functions like `is_fully_static`/`cast`/`assert_type` to `int | str` — which is wrong! — and which was probably at the core of the confusion here. Having this type as a parameter type is *useful* tough in order to *implement* these functions in Rust.
- Once we have `TypeForm`, we can make use of it to remove this hard-coded knowledge. Instead of receiving `int | str` directly in the Rust implementation, we would then receive `TypeForm[int | str]` and would need to unwrap that inner type first in order to implement those functions.
- @carljm pointed out that a possible route for us would be to implement `TypeForm` "right now", but this is something that we're going to discuss internally first.

---

_@InSyncWithFoo reviewed on 2025-01-08 20:56_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/src/types.rs`:3257 on 2025-01-08 20:56_

> Instead of receiving `int | str` directly in the Rust implementation, we would then receive `TypeForm[int | str]` and would need to unwrap that inner type first in order to implement those functions.

Shouldn't that be `types.UnionType & TypeForm[int | str]` instead? A similar example:

```python
print(type(type[int | str]))        # types.GenericAlias
print(type(type[int] | type[str]))  # types.UnionType

reveal_type(is_equivalent_to(type[int | str], type[int] | type[str]))
# True, `type` is distributive over unions.

# Some function in a runtime type inspection library
def f(v: types.UnionType): ...

f(type[int] | type[str])            # fine
f(type[int | str])                  # not fine
```


---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3257 on 2025-01-08 21:24_

> Shouldn't that be `types.UnionType & TypeForm[int | str]` instead

Sure, good observation, this would also be correct. In principle, inferring intersections could replace contextual type inference entirely, but in practice we won't want to try `infer_type_expression` blindly on every single expression in case maybe it could be a `TypeForm` type, so we'll want to use contextual inference simply for performance reasons.

We can make your example work just with contextual type inference, without inferring an intersection; if we see the argument `type[int] | type[str]` in a `types.UnionType` context we would infer it as `types.UnionType`, if we see it in a `TypeForm` context we infer it as `TypeForm[type[int] | type[str]]` (or equivalently `TypeForm[type[int | str]]`). If we are calling a function with an annotated parameter, it really doesn't matter, either way it will pass the assignable test, and the typing of the function's body will depend only on the annotated type anyway.

Inferring the intersection could be useful to get a more precise local inferred type for an annotated assignment, though. That way we could make code like this work:

```py
def takes_any_union(u: types.UnionType): ...
def takes_a_typeform(t: TypeForm[int | str]): ...

x: TypeForm = int | str  # we could infer `TypeForm[int | str]`, or `TypeForm[int | str] & types.UnionType` here

takes_a_typeform(x)
takes_any_union(x)  # this call will error unless we infer the intersection

```

Note that we can't just say in general that a `TypeForm` of a union type is always assignable to `types.UnionType`, because the union `TypeForm` could have originated from `A | B` or `typing.Union[A, B]`, which spell the same type (and thus result in the same `TypeForm[A | B]` type), but the former would be a `types.UnionType` and the latter would instead be a `typing._UnionGenericAlias`.

---

_@carljm reviewed on 2025-01-08 21:24_

---
