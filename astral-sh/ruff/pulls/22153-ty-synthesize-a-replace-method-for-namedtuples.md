```yaml
number: 22153
title: "[ty] Synthesize a `_replace` method for NamedTuples"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/syn
created_at: 2025-12-23T00:40:44Z
updated_at: 2025-12-23T21:33:57Z
url: https://github.com/astral-sh/ruff/pull/22153
synced_at: 2026-01-12T15:57:43Z
```

# [ty] Synthesize a `_replace` method for NamedTuples

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/2170.


---

_@charliermarsh reviewed on 2025-12-23 00:41_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:379 on 2025-12-23 00:41_

Mypy has the same behavior here. I'm not familiar enough to know whether this is the correct tradeoff, though.

---

_Comment by @astral-sh-bot[bot] on 2025-12-23 00:42_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-23 00:43_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5744:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataarray.py:5744:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
- xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5106 diagnostics
+ Found 5105 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14427 diagnostics
+ Found 14426 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2025-12-23 00:48_

---

_Review requested from @carljm by @charliermarsh on 2025-12-23 00:48_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-23 00:48_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-23 00:48_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-23 00:48_

---

_Label `ty` added by @charliermarsh on 2025-12-23 00:48_

---

_Renamed from "Synthesize a `_replace` method for NamedTuples" to "[ty ]Synthesize a `_replace` method for NamedTuples" by @charliermarsh on 2025-12-23 00:48_

---

_Renamed from "[ty ]Synthesize a `_replace` method for NamedTuples" to "[ty] Synthesize a `_replace` method for NamedTuples" by @charliermarsh on 2025-12-23 00:48_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:379 on 2025-12-23 01:26_

In my test it looks like mypy gets the best of both worlds? It seems to have the precise `_replace` signature but also allow the assignability: https://mypy-play.net/?mypy=latest&python=3.12&gist=0eafcba956567980dfaada547a91e2ce

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:379 on 2025-12-23 01:32_

It seems like mypy must be special-casing `_replace` on the protocol version of `NamedTuple` in order to achieve this, because it doesn't generally allow a similar protocol compatibility: https://mypy-play.net/?mypy=latest&python=3.12&gist=9ae6355cca81234089a0ea2d6a64b0f1

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:4706 on 2025-12-23 01:39_

It's not clear to me why this is correct or what case fails without it. For class-level access, the `self` argument should be explicitly supplied, I'm not sure why we should be doing anything extra here.

---

_@carljm reviewed on 2025-12-23 01:39_

---

_@charliermarsh reviewed on 2025-12-23 02:06_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types.rs`:4706 on 2025-12-23 02:06_

Without this change, we get the following:
```python
reveal_type(person._replace(name="Bob"))  # revealed: Self
```

---

_@charliermarsh reviewed on 2025-12-23 02:16_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:379 on 2025-12-23 02:16_

I feel like I'm going crazy because I definitely saw a diagnostic here from mypy that told me that this wasn't assignable (and it included the specific incompatibility). I must've been mistaken. Will revisit.

---

_@charliermarsh reviewed on 2025-12-23 02:31_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:379 on 2025-12-23 02:31_

I'm guessing my implementation isn't quite right, but I tried to now special-case `_replace` in protocol checking...

---

_@charliermarsh reviewed on 2025-12-23 02:34_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:379 on 2025-12-23 02:34_

Claude (reading mypy) claims that mypy treats `NamedTuple` specially as a nominal base type and bypasses method signature checking entirely?

https://github.com/python/mypy/blob/72cff30ce2d8db9fd6d8051b8c966ce9731c4fd4/mypy/subtypes.py#L531-L535

---

_@charliermarsh reviewed on 2025-12-23 02:47_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types.rs`:4706 on 2025-12-23 02:47_

Another variant that has the same effect:
```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index a6374a3d18..da46f40709 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -4695,22 +4695,14 @@ impl<'db> Type<'db> {
                 // method of these synthesized functions. The method-wrapper would then be returned from
                 // `find_name_in_mro` when called on function-like `Callable`s. This would allow us to
                 // correctly model the behavior of *explicit* `SomeDataclass.__init__.__get__` calls.
-                if instance.is_none(db) && callable.is_function_like(db) {
-                    // For class-level access (like Person._replace), we still want to replace
-                    // Self with the class's instance type (owner), but we don't bind the self parameter
-                    let owner_instance = owner.to_instance(db).unwrap_or(owner);
-                    let with_self_replaced = Type::Callable(CallableType::new(
-                        db,
-                        callable.signatures(db).apply_self(db, owner_instance),
-                        callable.kind(db),
-                    ));
-                    return Some((with_self_replaced, AttributeKind::NormalOrNonDataDescriptor));
-                }
-                // Pass the instance type so that `typing.Self` is replaced with the actual type
-                return Some((
-                    Type::Callable(callable.bind_self(db, Some(instance))),
-                    AttributeKind::NormalOrNonDataDescriptor,
-                ));
+                return if instance.is_none(db) && callable.is_function_like(db) {
+                    Some((self, AttributeKind::NormalOrNonDataDescriptor))
+                } else {
+                    Some((
+                        Type::Callable(callable.bind_self(db, None)),
+                        AttributeKind::NormalOrNonDataDescriptor,
+                    ))
+                };
             }
             _ => {}
         }
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index 71fef6825c..a2cb1047f4 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -2130,6 +2130,20 @@ impl<'db> ClassLiteral<'db> {
             }
         }
 
+        fn apply_self_to_callable<'d>(db: &'d dyn Db, ty: Type<'d>, self_instance: Type<'d>) -> Type<'d> {
+            match ty {
+                Type::Callable(callable_ty) if callable_ty.is_function_like(db) => {
+                    Type::Callable(callable_ty.apply_self(db, self_instance))
+                }
+                Type::Union(union) => {
+                    union.map(db, |element| apply_self_to_callable(db, *element, self_instance))
+                }
+                Type::Intersection(intersection) => intersection
+                    .map_positive(db, |element| apply_self_to_callable(db, *element, self_instance)),
+                _ => ty,
+            }
+        }
+
         let mut member = self.class_member_inner(db, None, name, policy);
 
         // We generally treat dunder attributes with `Callable` types as function-like callables.
@@ -2138,6 +2152,13 @@ impl<'db> ClassLiteral<'db> {
             member = member.map_type(|ty| into_function_like_callable(db, ty));
         }
 
+        // For callable members that use `Self` type variables (e.g., synthesized methods like
+        // `_replace` on NamedTuples), replace `Self` with the instance type of this class.
+        // This ensures that when accessing `Person._replace`, we get `(self: Person, ...) -> Person`
+        // instead of `(self: Self, ...) -> Self`.
+        let self_instance = Type::instance(db, ClassType::NonGeneric(self));
+        member = member.map_type(|ty| apply_self_to_callable(db, ty, self_instance));
+
         member
     }
```

---

_@carljm reviewed on 2025-12-23 03:02_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:379 on 2025-12-23 03:02_

Yeah, looks like Claude is right.

---

_@carljm reviewed on 2025-12-23 03:07_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:2165 on 2025-12-23 03:07_

What's not clear to me is why we are accessing `person._replace` on the class to begin with, when we are calling it on an instance. That's something we do for dunder methods (to mimic the runtime, which also does that), but it's not something we should do for a normal instance method call; `_replace` is not a dunder method. That seems to be at the root of the need for this.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/instance.rs`:144 on 2025-12-23 03:09_

I guess for the mypy equivalent we'd just totally short-circuit here and return true, since we've already done the work to check that we're comparing a named-tuple with `NamedTupleLike`. That seems... probably fine in practice? I'd want to see if @AlexWaygood knows of any reason we shouldn't just do that.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:2165 on 2025-12-23 03:13_

Or to put the same question in a more specific way: the previous version of the code had this special case in the branch where `instance` was `None` in the descriptor protocol. But that should not be the case for a call like `person._replace(...)`, where we clearly have an instance. So it seems like that's what we need to root-cause in order to fix this properly?

---

_@carljm reviewed on 2025-12-23 03:13_

---

_@charliermarsh reviewed on 2025-12-23 03:42_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/class.rs`:2165 on 2025-12-23 03:42_

With https://github.com/astral-sh/ruff/pull/22155, we now get the expected behavior for instances.

We still get `Self` for classes:

```python
reveal_type(Person._replace)  # revealed: (self: Self, *, name: str = ..., age: int | None = ...) -> Self
```

But perhaps that's expected?

---

_Closed by @AlexWaygood on 2025-12-23 09:57_

---

_Reopened by @AlexWaygood on 2025-12-23 09:58_

---

_Comment by @AlexWaygood on 2025-12-23 10:23_

I'd be inclined to revert the changes to `instance.rs` and just make the `NamedTupleLike` protocol in `ty_extensions.pyi` a little less precise. All tests pass with this change (except for the one that's modified slightly as part of the patch):

````diff
diff --git a/crates/ty_python_semantic/resources/mdtest/named_tuple.md b/crates/ty_python_semantic/resources/mdtest/named_tuple.md
index bf5b1e4d9b..812ff930c0 100644
--- a/crates/ty_python_semantic/resources/mdtest/named_tuple.md
+++ b/crates/ty_python_semantic/resources/mdtest/named_tuple.md
@@ -347,7 +347,7 @@ satisfy:
 def expects_named_tuple(x: typing.NamedTuple):
     reveal_type(x)  # revealed: tuple[object, ...] & NamedTupleLike
     reveal_type(x._make)  # revealed: bound method type[NamedTupleLike]._make(iterable: Iterable[Any]) -> NamedTupleLike
-    reveal_type(x._replace)  # revealed: bound method NamedTupleLike._replace(**kwargs) -> NamedTupleLike
+    reveal_type(x._replace)  # revealed: bound method NamedTupleLike._replace(...) -> NamedTupleLike
     # revealed: Overload[(value: tuple[object, ...], /) -> tuple[object, ...], (value: tuple[_T@__add__, ...], /) -> tuple[object, ...]]
     reveal_type(x.__add__)
     reveal_type(x.__iter__)  # revealed: bound method tuple[object, ...].__iter__() -> Iterator[object]
diff --git a/crates/ty_python_semantic/src/types/instance.rs b/crates/ty_python_semantic/src/types/instance.rs
index 11780e6fc6..9e674065b9 100644
--- a/crates/ty_python_semantic/src/types/instance.rs
+++ b/crates/ty_python_semantic/src/types/instance.rs
@@ -3,7 +3,6 @@
 use std::borrow::Cow;
 use std::marker::PhantomData;
 
-use super::class::CodeGeneratorKind;
 use super::protocol_class::ProtocolInterface;
 use super::{BoundTypeVarInstance, ClassType, KnownClass, SubclassOfType, Type, TypeVarVariance};
 use crate::place::PlaceAndQualifiers;
@@ -133,16 +132,6 @@ impl<'db> Type<'db> {
         relation_visitor: &HasRelationToVisitor<'db>,
         disjointness_visitor: &IsDisjointVisitor<'db>,
     ) -> ConstraintSet<'db> {
-        // Iff we're checking a NamedTuple against NamedTupleLike, skip the `_replace` method
-        // signature. NamedTuples synthesize `_replace` methods with specific keyword-only
-        // parameters (to detect invalid arguments), which are not strictly subtypes of the
-        // protocol's `(**kwargs)` signature, but are intended to be considered as satisfying it.
-        let is_namedtuple_protocol_check = matches!(&protocol.inner, Protocol::FromClass(class) if class.is_known(db, KnownClass::NamedTupleLike))
-            && self.as_nominal_instance().is_some_and(|instance| {
-                let (class_literal, specialization) = instance.class(db).class_literal(db);
-                CodeGeneratorKind::NamedTuple.matches(db, class_literal, specialization)
-            });
-
         let structurally_satisfied = if let Type::ProtocolInstance(self_protocol) = self {
             self_protocol.interface(db).has_relation_to_impl(
                 db,
@@ -157,16 +146,6 @@ impl<'db> Type<'db> {
                 .inner
                 .interface(db)
                 .members(db)
-                .filter(|member| {
-                    // Skip `_replace` check for NamedTuple vs NamedTupleLike. NamedTuples
-                    // synthesize `_replace` with specific keyword-only parameters to detect
-                    // invalid arguments, but this signature is not a strict subtype of the
-                    // protocol's `(**kwargs)` signature.
-                    if is_namedtuple_protocol_check && member.name() == "_replace" {
-                        return false;
-                    }
-                    true
-                })
                 .when_all(db, |member| {
                     member.is_satisfied_by(
                         db,
diff --git a/crates/ty_vendored/ty_extensions/ty_extensions.pyi b/crates/ty_vendored/ty_extensions/ty_extensions.pyi
index 347b6b4b34..ed0bf16186 100644
--- a/crates/ty_vendored/ty_extensions/ty_extensions.pyi
+++ b/crates/ty_vendored/ty_extensions/ty_extensions.pyi
@@ -190,6 +190,16 @@ class NamedTupleLike(Protocol):
     @classmethod
     def _make(cls: type[Self], iterable: Iterable[Any]) -> Self: ...
     def _asdict(self, /) -> dict[str, Any]: ...
-    def _replace(self, /, **kwargs) -> Self: ...
+
+    # Positional arguments aren't actually accepted by these methods at runtime,
+    # but adding the `*args` parameters means that all `NamedTuple` classes
+    # are understood as assignable to this protocol due to the special case
+    # outlined in https://typing.python.org/en/latest/spec/callables.html#meaning-of-in-callable:
+    #
+    # > If the input signature in a function definition includes both a
+    # > `*args` and `**kwargs` parameter and both are typed as `Any`
+    # > (explicitly or implicitly because it has no annotation), a type
+    # > checker should treat this as the equivalent of `...`.
+    def _replace(self, *args, **kwargs) -> Self: ...
     if sys.version_info >= (3, 13):
-        def __replace__(self, **kwargs) -> Self: ...
+        def __replace__(self, *args, **kwargs) -> Self: ...
````

---

_Comment by @AlexWaygood on 2025-12-23 10:35_

On Python 3.13+, it looks like we should also be synthesising precise `__replace__` methods for `NamedTuple` classes in the same way that we already do for dataclasses.

---

_Review requested from @MichaReiser by @charliermarsh on 2025-12-23 14:00_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/overrides.rs`:39 on 2025-12-23 15:21_

What's the reason for this change? Overriding `__replace__` doesn't seem to cause a runtime error in the same way that overriding `_replace` does:

```pycon
Python 3.13.1 (main, Jan  3 2025, 12:04:03) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from typing import NamedTuple
>>> class Foo(NamedTuple):
...     __replace__ = None
...     
>>> class Foo(NamedTuple):
...     _replace = None
...     
Traceback (most recent call last):
  File "<python-input-2>", line 1, in <module>
    class Foo(NamedTuple):
        _replace = None
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 3021, in __new__
    raise AttributeError("Cannot overwrite NamedTuple attribute " + key)
AttributeError: Cannot overwrite NamedTuple attribute _replace
```

This change also doesn't seem to be tested at all?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2524 on 2025-12-23 15:24_

nit

```suggestion
                if matches!(name, "__replace__" | "_replace") {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2511 on 2025-12-23 15:24_

nit

```suggestion
                    matches!(name, "__replace__" | "_replace") || kw_only.unwrap_or(false);
```

---

_@AlexWaygood approved on 2025-12-23 15:28_

LGTM

---

_Comment by @charliermarsh on 2025-12-23 16:14_

(Depends on https://github.com/astral-sh/ruff/pull/22155.)

---

_Merged by @charliermarsh on 2025-12-23 21:33_

---

_Closed by @charliermarsh on 2025-12-23 21:33_

---

_Branch deleted on 2025-12-23 21:33_

---
