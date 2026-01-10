```yaml
number: 20328
title: "[ty] Patch `Self` for fallback-methods on `NamedTuple`s and `TypedDict`s"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/patch-self-on-fallback-types
created_at: 2025-09-10T12:44:34Z
updated_at: 2025-09-15T14:38:30Z
url: https://github.com/astral-sh/ruff/pull/20328
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Patch `Self` for fallback-methods on `NamedTuple`s and `TypedDict`s

---

_Pull request opened by @sharkdp on 2025-09-10 12:44_

## Summary

We use classes like [`_typeshed._type_checker_internals.NamedTupleFallback`](https://github.com/python/typeshed/blob/d9c76e1d9f0c0000c1971c3e936a69dd55f49ce1/stdlib/_typeshed/_type_checker_internals.pyi#L54-L75) to tack on additional attributes/methods to instances of user-defined `NamedTuple`s (or `TypedDict`s), even though these classes are not present in the MRO of those types.

The problem is that those classes use implicit and explicit `Self` annotations which refer to `NamedTupleFallback` itself, instead of to the actual type that we're adding those methods to:
```py
class NamedTupleFallback(tuple[Any, ...]):
    # […]
    def _replace(self, **kwargs: Any) -> typing_extensions.Self: ...
```

In effect, when we access `_replace` on an instance of a custom `NamedTuple` instance, its `self` parameter and return type refer to the wrong `Self`. This leads to incorrect *"Argument to bound method `_replace` is incorrect: Argument type `Person` does not satisfy upper bound `NamedTupleFallback` of type variable `Self`"* errors on #18007. It would also lead to similar errors on `TypedDict`s, if they would already implement assignability properly.


## Test Plan

I applied the following patch to typeshed and verified that no errors appear anymore.

<details>

```diff
diff --git a/crates/ty_vendored/vendor/typeshed/stdlib/_typeshed/_type_checker_internals.pyi b/crates/ty_vendored/vendor/typeshed/stdlib/_typeshed/_type_checker_internals.pyi
index feb22aae00..8e41034f19 100644
--- a/crates/ty_vendored/vendor/typeshed/stdlib/_typeshed/_type_checker_internals.pyi
+++ b/crates/ty_vendored/vendor/typeshed/stdlib/_typeshed/_type_checker_internals.pyi
@@ -29,27 +29,27 @@ class TypedDictFallback(Mapping[str, object], metaclass=ABCMeta):
         __readonly_keys__: ClassVar[frozenset[str]]
         __mutable_keys__: ClassVar[frozenset[str]]
 
-    def copy(self) -> typing_extensions.Self: ...
+    def copy(self: typing_extensions.Self) -> typing_extensions.Self: ...
     # Using Never so that only calls using mypy plugin hook that specialize the signature
     # can go through.
-    def setdefault(self, k: Never, default: object) -> object: ...
+    def setdefault(self: typing_extensions.Self, k: Never, default: object) -> object: ...
     # Mypy plugin hook for 'pop' expects that 'default' has a type variable type.
-    def pop(self, k: Never, default: _T = ...) -> object: ...  # pyright: ignore[reportInvalidTypeVarUse]
-    def update(self, m: typing_extensions.Self, /) -> None: ...
-    def __delitem__(self, k: Never) -> None: ...
-    def items(self) -> dict_items[str, object]: ...
-    def keys(self) -> dict_keys[str, object]: ...
-    def values(self) -> dict_values[str, object]: ...
+    def pop(self: typing_extensions.Self, k: Never, default: _T = ...) -> object: ...  # pyright: ignore[reportInvalidTypeVarUse]
+    def update(self: typing_extensions.Self, m: typing_extensions.Self, /) -> None: ...
+    def __delitem__(self: typing_extensions.Self, k: Never) -> None: ...
+    def items(self: typing_extensions.Self) -> dict_items[str, object]: ...
+    def keys(self: typing_extensions.Self) -> dict_keys[str, object]: ...
+    def values(self: typing_extensions.Self) -> dict_values[str, object]: ...
     @overload
-    def __or__(self, value: typing_extensions.Self, /) -> typing_extensions.Self: ...
+    def __or__(self: typing_extensions.Self, value: typing_extensions.Self, /) -> typing_extensions.Self: ...
     @overload
-    def __or__(self, value: dict[str, Any], /) -> dict[str, object]: ...
+    def __or__(self: typing_extensions.Self, value: dict[str, Any], /) -> dict[str, object]: ...
     @overload
-    def __ror__(self, value: typing_extensions.Self, /) -> typing_extensions.Self: ...
+    def __ror__(self: typing_extensions.Self, value: typing_extensions.Self, /) -> typing_extensions.Self: ...
     @overload
-    def __ror__(self, value: dict[str, Any], /) -> dict[str, object]: ...
+    def __ror__(self: typing_extensions.Self, value: dict[str, Any], /) -> dict[str, object]: ...
     # supposedly incompatible definitions of __or__ and __ior__
-    def __ior__(self, value: typing_extensions.Self, /) -> typing_extensions.Self: ...  # type: ignore[misc]
+    def __ior__(self: typing_extensions.Self, value: typing_extensions.Self, /) -> typing_extensions.Self: ...  # type: ignore[misc]
 
 # Fallback type providing methods and attributes that appear on all `NamedTuple` types.
 class NamedTupleFallback(tuple[Any, ...]):
@@ -61,18 +61,18 @@ class NamedTupleFallback(tuple[Any, ...]):
         __orig_bases__: ClassVar[tuple[Any, ...]]
 
     @overload
-    def __init__(self, typename: str, fields: Iterable[tuple[str, Any]], /) -> None: ...
+    def __init__(self: typing_extensions.Self, typename: str, fields: Iterable[tuple[str, Any]], /) -> None: ...
     @overload
     @typing_extensions.deprecated(
         "Creating a typing.NamedTuple using keyword arguments is deprecated and support will be removed in Python 3.15"
     )
-    def __init__(self, typename: str, fields: None = None, /, **kwargs: Any) -> None: ...
+    def __init__(self: typing_extensions.Self, typename: str, fields: None = None, /, **kwargs: Any) -> None: ...
     @classmethod
     def _make(cls, iterable: Iterable[Any]) -> typing_extensions.Self: ...
-    def _asdict(self) -> dict[str, Any]: ...
-    def _replace(self, **kwargs: Any) -> typing_extensions.Self: ...
+    def _asdict(self: typing_extensions.Self) -> dict[str, Any]: ...
+    def _replace(self: typing_extensions.Self, **kwargs: Any) -> typing_extensions.Self: ...
     if sys.version_info >= (3, 13):
-        def __replace__(self, **kwargs: Any) -> typing_extensions.Self: ...
+        def __replace__(self: typing_extensions.Self, **kwargs: Any) -> typing_extensions.Self: ...
 
 # Non-default variations to accommodate couroutines, and `AwaitableGenerator` having a 4th type parameter.
 _S = TypeVar("_S")
```

</details>

---

_Label `ty` added by @sharkdp on 2025-09-10 12:44_

---

_Comment by @github-actions[bot] on 2025-09-10 12:46_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-10 12:49_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-10 12:50_

---

_Comment by @github-actions[bot] on 2025-09-10 12:54_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://david-patch-self-on-fallback.ecosystem-663.pages.dev/diff)**


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-12 12:43_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-12 12:43_

---

_Renamed from "[ty] Patch (implicit) `Self` on fallback types" to "[ty] Patch `Self` on fallback types" by @sharkdp on 2025-09-12 12:57_

---

_Renamed from "[ty] Patch `Self` on fallback types" to "[ty] Patch `Self` for fallback-methods on `NamedTuple`s and `TypedDict`s" by @sharkdp on 2025-09-12 13:26_

---

_Marked ready for review by @sharkdp on 2025-09-12 13:27_

---

_Review requested from @carljm by @sharkdp on 2025-09-12 13:27_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-12 13:27_

---

_Review requested from @dcreager by @sharkdp on 2025-09-12 13:27_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:10907 on 2025-09-12 14:01_

I think you can use `take_while` to simplify this:

```suggestion
    let upper_bound = class_literal
        .iter_mro(db, specialization)
        .take_while(|base| !is_known_base(base))
        .filter_map(ClassBase::into_class)
        .last()
        .unwrap_or_else(|| class_literal.unknown_specialization(db));
    Type::instance(db, upper_bound)
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:273 on 2025-09-12 14:09_

This loses the `@_replace` suffix because you give the newly synthesized typevar a binding context of `BindingContext::Synthesized`. I think it would be better to copy the binding context over from the `NamedTupleFallback` method. (The binding context is primarily used to differentiate between different uses of the same legacy typevar, so I don't think anything will _break_ without this change, but I think it's (a) more correct and (b) not substantially more difficult to implement, so I think it's worth doing.)

To do this, you'd need to update your new `TypeMapping::ReplaceSelf` to take in the new upper bound, rather than a fully constructed `BoundTypeVarInstance`, so that you could copy over all of the other fields from the typevar being replaced.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:492 on 2025-09-12 14:11_

:+1: You might consider a new `TypeMapping::updates_generic_context` method or similar (which would currently only return true for `ReplaceSelf`), and use that in the `if` check. That would hopefully make it more obvious how to add any future type mappings that also need to engage this new logic.

---

_@dcreager reviewed on 2025-09-12 14:12_

---

_@sharkdp reviewed on 2025-09-12 14:24_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/signatures.rs`:492 on 2025-09-12 14:24_

Yes, good suggestion. There might be a second type mapping that also needs to modify the generic context (as you mentioned in one of our chats, `BindSelf` should remove `Self` from the generic context, see https://github.com/astral-sh/ruff/pull/20364).

---

_Review request for @carljm removed by @carljm on 2025-09-12 21:41_

---

_Review requested from @dcreager by @sharkdp on 2025-09-15 11:32_

---

_Comment by @sharkdp on 2025-09-15 14:21_

I will merge this without approval, because I want to have an easier way of rebasing https://github.com/astral-sh/ruff/pull/18007 on this. Happy to address any post-merge comments.

---

_Merged by @sharkdp on 2025-09-15 14:21_

---

_Closed by @sharkdp on 2025-09-15 14:21_

---

_Branch deleted on 2025-09-15 14:21_

---

_Comment by @dcreager on 2025-09-15 14:38_

> I will merge this without approval

:+1: Looks good!

---
