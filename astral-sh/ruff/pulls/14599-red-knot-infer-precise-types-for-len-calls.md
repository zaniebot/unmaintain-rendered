```yaml
number: 14599
title: "[red-knot] Infer precise types for `len()` calls"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-len
created_at: 2024-11-26T04:55:15Z
updated_at: 2024-12-04T19:29:41Z
url: https://github.com/astral-sh/ruff/pull/14599
synced_at: 2026-01-10T20:42:26Z
```

# [red-knot] Infer precise types for `len()` calls

---

_Pull request opened by @InSyncWithFoo on 2024-11-26 04:55_

## Summary

Resolves #14598.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2024-11-26 04:55_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2024-11-26 04:55_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2024-11-26 04:55_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2024-11-26 04:55_

---

_Comment by @github-actions[bot] on 2024-11-26 05:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @MichaReiser on 2024-11-26 06:44_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:3078 on 2024-11-26 06:50_

```suggestion
    pub fn chars_count(&self, db: &'db dyn Db) -> usize {
        self.value(db).chars().count()
    }
```

The name here is confusing because `str.len` `chars().count` is the number of characters. The utf8-len is `str.len`. 

I suggest to either rename the method to `chars_count` or expose a `as_str` method that returns the str instead.

---

_@MichaReiser reviewed on 2024-11-26 06:50_

---

_@sharkdp reviewed on 2024-11-26 07:58_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2472 on 2024-11-26 07:58_

Minor point: maybe rename this to `StaticallyUnknown` or similar to avoid any confusion with `Type::Unknown`? (especially since you map `Len::Never` to `Type::Never`)

---

_@sharkdp reviewed on 2024-11-26 08:12_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2474 on 2024-11-26 08:12_

I understand the intention behind returning `Type::Never` in cases where we can statically see that the `len` call will return an error at runtime. But I'm not sure if this leads to the best user experience?

Once we have call signature checking, we will definitely raise a diagnostic for something like taking the `len` of a `Literal[1]`/`int` here:
```py
x = 1
xs = [x]

# some code

n = len(x)  # oops, was supposed to be `xs` <-- we will definitely raise a diagnostic here

# more code
```

The question is what happens after the `n = len(x)` line. If we infer `Never`, we risk hiding further downstream errors. If we infer `int` (because we know that the error in the offending line has to be fixed anyway), we can catch other potential errors in the `more code` section.

Once we add unreachable-code analysis, we might even flag the `more code` section as unreachable if we infer `Never` here. Which is technically correct, but potentially confusing if all you need to do is to fix the `len(x)` error.

---

_@sharkdp reviewed on 2024-11-26 08:30_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1394 on 2024-11-26 08:30_

We fall back to a hard-coded `int` here in case we can't statically compute a more precise type. I think I would prefer a slightly different logic here where we try to compute a statically-known result type, but if that fails, fall back to normal signature checking, like we do in the catchall `_ =>` branch below. This would lead to [the same result](https://github.com/python/typeshed/blob/02533d13ea43423fab76f6e8d2decf1350fcc302/stdlib/builtins.pyi#L1480) under normal circumstances, but would also handle cases where we have a custom `typeshed`, for example.

---

_@InSyncWithFoo reviewed on 2024-11-26 10:14_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/src/types.rs`:2474 on 2024-11-26 10:14_

Removed `Len::Never`. I was thinking about cases where a `len(...) == ...` might affect narrowing; in hindsight, that does seem overcomplicated.

```python
if len(a) == 2:           # Never != 2
    do_something_with(a)  # Unreachable
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/expression/len.md`:146 on 2024-11-26 20:16_

```suggestion
### Negative literal integer
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/expression/len.md`:187 on 2024-11-26 20:16_

```suggestion
    # TODO emit a diagnostic
    def __len__(self) -> Literal[-1]: ...
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/expression/len.md`:94 on 2024-11-26 20:23_

That's fine, but we should also have a test for the simple and more common case where `__len__` is annotated to return an int literal directly. I think that case wouldn't be that hard to support in this PR, rather than leaving it as a todo; in a sense it's the most basic case we should support.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/expression/len.md`:188 on 2024-11-26 20:24_

To be honest I am not sure if it will ever be worth supporting all this, rather than just inferring `int`. But we can leave it as a Todo and see if we ever prioritize it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1397 on 2024-11-26 22:23_

We should not have special handling for the `Todo` type as a tuple element, even if this gives us wrong results currently in some cases with starred elements. We can mark those cases with TODOs in the tests. They will be fixed when we fix the todo in tuple construction.

```suggestion
            Type::Tuple(tuple) => tuple.elements(db).len().into(),
```

---

_@carljm requested changes on 2024-11-26 22:25_

Thank you!! A few comments.

---

_@InSyncWithFoo reviewed on 2024-11-27 08:21_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/expression/len.md`:188 on 2024-11-27 08:21_

I didn't think so either; these were added just for the sake of completeness. Added a few more edge cases in the same vein.

---

_Review requested from @carljm by @InSyncWithFoo on 2024-11-27 08:24_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1375 on 2024-11-27 17:07_

This union can't exist, it would be simplified to just `int`.
```suggestion
```

On the other hand, a case I think we will need handling for is the case where there's a literal boolean in a union, e.g. `Literal[1, True]`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2482 on 2024-11-27 17:24_

I'm hesitant to leave this as a TODO, because it's odd to support some valid return annotations on `__len__` and not others (so I think we'll want it), and it's not particularly complex to implement, but it could have significant impact on the overall design of this PR. For instance, if we store a vec of u64, that means `Len` can no longer be `Copy`. It might suggest alternative designs, like `Len` wrapping a `Type` (which is interned, thus `Copy`), or perhaps just having `Type::len()` return a `Type`. It's not clear to me that we'll get enough value from the `Len` enum to justify the extra ceremony of converting a `Type` to a `Len` just so we can (at least in the only use case in this PR) immediately convert it back to a `Type`.

Though we do need to validate the type from `__len__`: anything not assignable to `builtins.int` should fall back to `builtins.int` (and emit a diagnostic on the `__len__` definition, though that doesn't have to be part of this PR), and we do need the special-case to convert a `BooleanLiteral` to an `IntLiteral` (including inside of a union). So we can't simplify down to a simple pass-through from the return type of `__len__`.

---

_@carljm reviewed on 2024-11-28 03:21_

---

_@AlexWaygood reviewed on 2024-11-28 11:59_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2482 on 2024-11-28 11:59_

> or perhaps just having `Type::len()` return a `Type`.

This seems like the most obvious design to me. I think I'd probably have have `Type::len` return `Option<Type<'db>>`. Or if there are multiple things that can go wrong when calling `len(x)`, then you could it return `Result<Type<'db>, LenError>`, where `LenError` is an enum that lists the various ways calling `len(x)` can result in an error at runtime

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/src/types.rs`:2482 on 2024-11-28 17:26_

Done: I added support for unions and made `Type::len()` return `Option<Type<'db>>`.

---

_@InSyncWithFoo reviewed on 2024-11-28 17:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2596 on 2024-11-28 17:29_

I would prefer that we enumerate these all explicitly. It means that the compiler forces us to think about whether the new function we're adding support for is a constraint function or not whenever we add a new `KnownFunction`; this is very useful!

```suggestion
            Self::RevealType | Self::Len => None,
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1377 on 2024-11-28 17:44_

I think you've correctly identified here that `StringLiteralType::len` would be subtly incorrect for your use case here. However, `StringLiteralType::len` is only used in one place currently, and I think its implementation is subtly incorrect for _that_ use case as well. It's used here, in our implementation of unpacked assignments:

https://github.com/astral-sh/ruff/blob/d9cbf2fe44e3f13b28295377a41ae541cab6b54f/crates/red_knot_python_semantic/src/types/unpacker.rs#L90-L101

But according to the [Rust docs for `str`](https://doc.rust-lang.org/std/primitive.str.html#method.len), `"ƒoo".len() == 4` in Rust (because of the fancy Unicode `f`). That gives us the wrong result for Python unpacking:

```pycon
>>> a, b, c = "ƒoo"
>>> a
'ƒ'
>>> b
'o'
>>> c
'o'
```

Therefore, I think rather than adding a separate `StringLiteralType::as_str()`, we should simply change `StringLiteralType::len` to count the characters rather than the bytes (and add a comment about why). Per Micha's earlier comment, it might be best to rename it to `python_len()`, so that we don't get confused about how it differs from the way that Rust counts the length of the string

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1383 on 2024-11-28 17:48_

this is incorrect -- we want to treat classes the same as instances here (because a class is an instance of its metaclass). I think you want something like

```rs
match self {
    Type::BytesLiteral(bytes) => return bytes.len(db).try_into().ok(),
    Type::StringLiteral(string) => {
        return string.as_str(db).chars().count().try_into().ok()
    }
    Type::Tuple(tuple) => tuple.elements(db).len().try_into().ok(),
    _ => {}
}

let meta_type = self.to_meta_type();

let Symbol::Type(dunder_len, _) = meta_type.member(db, "__len__") else {
    return None;
};
```

We do something similar here for our `Type::iter()` method; you should be able to look at that for inspiration

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1394 on 2024-11-28 17:51_

It doesn't actually need to be a function; any callable object will do:

```pycon
>>> class Foo:
...     def __call__(self):
...         return 42
...         
>>> class Bar:
...     __len__ = Foo()
...     
>>> len(Bar())
42
```

We have a `Type::call()` method that abstracts away the details of trying to call a possibly-not-callable type; you should be able to use that here, which will mean you'll support arbitrary callable objects here rather than just functions

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3047 on 2024-11-28 17:53_

This is cool, but doesn't seem to be used right now; do we need it?

```suggestion
```

---

_@AlexWaygood had review dismissed on 2024-11-28 17:54_

Thanks!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:445 on 2024-11-28 19:22_

I'm not a massive fan of this. Converting "from a usize to a Type" doesn't feel like the right way of thinking about what we're doing when we attempt to create a `Type::IntLiteral` from a numeric value that has type `usize`. It also doesn't save us many lines; I think I'd prefer to just write it out:

```diff
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -1,5 +1,4 @@
 use std::hash::Hash;
-use std::num::TryFromIntError;
 
 use indexmap::IndexSet;
 use itertools::Itertools;
@@ -436,14 +435,6 @@ pub enum Type<'db> {
     // TODO protocols, callable types, overloads, generics, type vars
 }
 
-impl TryFrom<usize> for Type<'_> {
-    type Error = TryFromIntError;
-
-    fn try_from(value: usize) -> Result<Self, Self::Error> {
-        i64::try_from(value).map(Type::IntLiteral)
-    }
-}
-
 impl<'db> Type<'db> {
     pub const fn is_never(&self) -> bool {
         matches!(self, Type::Never)
@@ -1371,11 +1362,15 @@ impl<'db> Type<'db> {
     /// This is used to determine the value that would be returned
     /// when `len(x)` is called on an object `x`.
     fn len(&self, db: &'db dyn Db) -> Option<Type<'db>> {
-        match self {
-            Type::BytesLiteral(bytes) => return bytes.len(db).try_into().ok(),
-            Type::StringLiteral(string) => return string.python_len(db).try_into().ok(),
-            Type::Tuple(tuple) => return tuple.elements(db).len().try_into().ok(),
-            _ => {}
+        let usize_len = match self {
+            Type::BytesLiteral(bytes) => Some(bytes.len(db)),
+            Type::StringLiteral(string) => Some(string.python_len(db)),
+            Type::Tuple(tuple) => Some(tuple.elements(db).len()),
+            _ => None,
+        };
+
+        if let Some(usize_len) = usize_len {
+            return usize_len.try_into().ok().map(Type::IntLiteral);
         }
```

---

_@AlexWaygood reviewed on 2024-11-28 19:22_

---

_@dhruvmanila reviewed on 2024-11-29 05:04_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/unpacker.rs`:98 on 2024-11-29 05:04_

Thanks for catching this. Can we add a test for this in `unpacking.md`? Like:
```py
(a, b) = "é"

reveal_type(a)  # revealed: LiteralString
reveal_type(b)  # revealed: Unknown
```

---

_@InSyncWithFoo reviewed on 2024-11-29 05:40_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/src/types/unpacker.rs`:98 on 2024-11-29 05:40_

Sure. I added a few Unicode tests.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:533 on 2024-11-29 11:12_

While I can see that it's quite convenient, I think this change is a little bit dangerous.

Firstly, it's inconsistent with all the other `into_*` methods we have on `Type`: the pattern for these methods is that if the `Type` instance has exactly the variant we expect, the method returns `Some(<wrapped inner data>)`, but for any other variant the method returns `None`. This method breaks that pattern.

Secondly, it's incorrect from the perspective of Python's type system in some cases. Sometimes we can treat a boolean literal equivalently to a int literal, and I think you're correct that this is so for all the cases in this PR. But it's not always the case that you can do that: `Literal[True]` is _not_ a subtype of `Literal[1]`, despite `True` and `1` both comparing equal at runtime, and despite `bool` being a subtype of `int`. For example, in due course we will need to issue a diagnostic on something like this:

```py
from typing import Literal

def foo(x: Literal[1, 2]) -> None:
    ...

foo(True)
```

For these reasons, I would not make the change you're making to this method, and I would not write `as_int_literal` the way you do below. In fact, since you only use the `as_len_int_literal` method once, inside `Type::len`, I'd probably inline the method there. Something like this (relative to your PR):

```diff
diff --git a/crates/red_knot_python_semantic/src/types.rs b/crates/red_knot_python_semantic/src/types.rs
index 83cd9b6d6..f2eba093e 100644
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -527,7 +527,6 @@ impl<'db> Type<'db> {
     pub const fn into_int_literal(self) -> Option<i64> {
         match self {
             Type::IntLiteral(value) => Some(value),
-            Type::BooleanLiteral(value) => Some(if value { 1 } else { 0 }),
             _ => None,
         }
     }
@@ -551,22 +550,6 @@ impl<'db> Type<'db> {
             .expect("Expected a Type::KnownInstance variant")
     }
 
-    pub fn as_int_literal(&self) -> Option<Type<'db>> {
-        match self {
-            int_literal @ Type::IntLiteral(_) => Some(*int_literal),
-            Type::BooleanLiteral(value) => Some(Type::IntLiteral(i64::from(*value))),
-            _ => None,
-        }
-    }
-
-    /// Returns the result of [`Type::as_int_literal`] if that value is not negative.
-    pub fn as_len_int_literal(&self) -> Option<Type<'db>> {
-        self.as_int_literal().take_if(|it| match it {
-            Type::IntLiteral(value) => *value >= 0,
-            _ => false,
-        })
-    }
-
     pub const fn is_boolean_literal(&self) -> bool {
         matches!(self, Type::BooleanLiteral(..))
     }
@@ -1362,6 +1345,22 @@ impl<'db> Type<'db> {
     /// This is used to determine the value that would be returned
     /// when `len(x)` is called on an object `x`.
     fn len(&self, db: &'db dyn Db) -> Option<Type<'db>> {
+        fn positive_int_literal<'db>(db: &'db dyn Db, ty: Type<'db>) -> Option<Type<'db>> {
+            match ty {
+                // TODO emit diagnostic for Literal integer <0
+                Type::IntLiteral(int) => (int >= 0).then_some(ty),
+                Type::BooleanLiteral(bool_literal) => Some(Type::IntLiteral(bool_literal.into())),
+                Type::Union(union) => {
+                    let mut builder = UnionBuilder::new(db);
+                    for element in union.elements(db) {
+                        builder = builder.add(positive_int_literal(db, *element)?);
+                    }
+                    Some(builder.build())
+                }
+                _ => None,
+            }
+        }
+
         let usize_len = match self {
             Type::BytesLiteral(bytes) => Some(bytes.len(db)),
             Type::StringLiteral(string) => Some(string.python_len(db)),
@@ -1380,21 +1379,7 @@ impl<'db> Type<'db> {
             }
         };
 
-        let Type::Union(union) = return_ty else {
-            return return_ty.as_len_int_literal();
-        };
-
-        let converted = union
-            .elements(db)
-            .iter()
-            .map(|e| e.as_len_int_literal().unwrap_or(Type::Unknown))
-            .collect::<Vec<_>>();
-
-        if converted.iter().any(|e| matches!(e, Type::Unknown)) {
-            return None;
-        }
-
-        Some(UnionType::from_elements(db, converted))
+        positive_int_literal(db, return_ty)
     }

```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/expression/len.md`:188 on 2024-11-29 11:18_

I think the key reason why inferring precise types for `len()` comes in handy is so that you can narrow `tuple[int, ...]` to `tuple[int, int]` by testing the length of the tuple. I'm not sure that inferring the precise length of an arbitrary instance holds that much value, so I also doubt it will ever be worth supporting this really.

---

_@AlexWaygood reviewed on 2024-11-29 11:27_

---

_@AlexWaygood reviewed on 2024-11-29 14:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3036 on 2024-11-29 14:35_

```suggestion
        self.value(db).chars().count()
```

---

_@AlexWaygood reviewed on 2024-11-29 14:38_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1401 on 2024-11-29 14:38_

```suggestion
                    let [only_arg] = arg_types else {
                        // TODO should really return a variant indicating the wrong
                        // number of arguments was passed (we don't have that yet)
                        return CallOutcome::callable(normal_return_ty);
                    };
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/expression/len.md`:59 on 2024-12-04 17:47_

Let's be a bit more specific on the TODOs here, to help make things clear for future-us updating this later:
```suggestion
# TODO: Handle star unpacks, should be Literal[0]
reveal_type(len((*[],)))  # revealed: Literal[1]
# TODO: Handle star unpacks, should be Literal[1]
reveal_type(  # revealed: Literal[2]
    len(
        (
            *[],
            1,
        )
    )
)
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/expression/len.md`:61 on 2024-12-04 17:48_

```suggestion
# TODO: handle star unpacks, should be Literal[2]
reveal_type(len((*[], 1, 2)))  # revealed: Literal[3]
# TODO: handle star unpacks, should be Literal[0]
reveal_type(len((*[], *{})))  # revealed: Literal[2]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/expression/len.md`:118 on 2024-12-04 17:50_

Both of these should emit a diagnostic.
```suggestion
# TODO: diagnostic
reveal_type(len(OneOrFoo()))  # revealed: int
# TODO: diagnostic
reveal_type(len(ZeroOrStr()))  # revealed: int
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/expression/len.md`:180 on 2024-12-04 17:53_

On looking at this again, I think we probably shouldn't emit the diagnostic on the definition of `__len__` with an invalid return type, but rather when `len()` is called on it. It's actually `len()` that enforces the restriction, this code doesn't raise any error at runtime:

```py
class A:
    def __len__(self):
        return -1

A.__len__()
```

It's only `len(A())` that errors. So I think erroring on the definition of `__len__` here would be more like a lint rule, it shouldn't be a core type checker error.

```suggestion
class Negative:
    def __len__(self) -> Literal[-1]: ...

# TODO: diagnostic
reveal_type(len(Negative()))  # revealed: int
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/expression/len.md`:197 on 2024-12-04 17:55_

Same as above.
```suggestion
class SecondOptionalArgument:
    def __len__(self, v: int = 0) -> Literal[0]: ...

class SecondRequiredArgument:
    def __len__(self, v: int) -> Literal[1]: ...

# TODO: diagnostic
reveal_type(len(SecondOptionalArgument()))  # revealed: Literal[0]
# TODO: diagnostic
reveal_type(len(SecondRequiredArgument()))  # revealed: Literal[1]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1350 on 2024-12-04 17:57_

```suggestion
                // TODO: Emit diagnostic for non-integers, or negative integers
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1346 on 2024-12-04 17:59_

```suggestion
    /// Return the type of `len()` on a type, if it is known more precisely than `int`.
    ///
    /// Return `None` if it isn't known precisely; in this case we fall back to the return type of `len()`
    /// in typeshed, which is `int`.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1455 on 2024-12-04 18:05_

Let's also add a test for this case (user class with no defined `__len__`), showing that we get `int` and with a `TODO` for a diagnostic in the test also.

```suggestion
            # TODO: emit a diagnostic
            CallDunderResult::MethodNotAvailable => return None,
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1401 on 2024-12-04 18:05_

```suggestion
                        // TODO: diagnostic
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3049 on 2024-12-04 18:07_

Should this method also be named `python_len` in parallel with the method on `StringLiteralType`?

---

_@carljm reviewed on 2024-12-04 18:11_

This looks quite close! Thanks for your patience with all the review comments.

I think it makes sense to leave the diagnostics as TODO here, because it will be a lot easier to emit these diagnostics once https://github.com/astral-sh/ruff/pull/14760 lands (we won't have to return some indicator such that inference emits the diagnostic, we'll be able to just emit it directly where we find the issue.)

---

_@InSyncWithFoo reviewed on 2024-12-04 18:29_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/src/types.rs`:3049 on 2024-12-04 18:29_

I'm not sure about this. Won't the length of a byte sequence be the same in both Python and Rust?

---

_Comment by @MichaReiser on 2024-12-04 18:30_

@carljm I'm not sure if #14760 will land. It's unclear to me how suppressions would work with accumulators whereas it's clear to me how they would work in our existing model.

---

_@carljm reviewed on 2024-12-04 18:31_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3049 on 2024-12-04 18:31_

Yes. But the point is clarity to the reader that this is the case, and that there isn't a missing method for getting the Python length of a byte sequence, the correct way is the `python_len` method. We could add `python_len` and just have it be a pass-through to `len()`, but if we don't currently need a way to get the Rust length, that doesn't seem necessary.

---

_Comment by @carljm on 2024-12-04 18:35_

@MichaReiser Ok, let's discuss those details elsewhere. The only part that's relevant here is to say that I think accumulators will be super useful in type checking and make a lot of things much simpler and easier, so I think we really want/need it, even if it requires more work on handling suppressions.

---

_@carljm reviewed on 2024-12-04 19:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1421 on 2024-12-04 19:01_

```suggestion
    /// Return the type of `len()` on a type if it is known more precisely than `int`,
    /// or `None` otherwise.
```

---

_@carljm reviewed on 2024-12-04 19:02_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1421 on 2024-12-04 19:02_

```suggestion
    /// Return the type of `len()` on a type if it is known more precisely than `int`,
    /// or `None` otherwise.
```

---

_@carljm approved on 2024-12-04 19:13_

---

_Comment by @carljm on 2024-12-04 19:16_

Thanks! And great find on the string-literal-unpacking issue. Merging.

---

_Merged by @carljm on 2024-12-04 19:16_

---

_Closed by @carljm on 2024-12-04 19:16_

---

_Branch deleted on 2024-12-04 19:29_

---
