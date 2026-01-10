```yaml
number: 1048
title: "BUG: false positive in `df.attrs.update()`"
type: issue
state: closed
author: janosh
labels: []
assignees: []
created_at: 2025-08-19T06:12:54Z
updated_at: 2025-08-20T12:55:28Z
url: https://github.com/astral-sh/ty/issues/1048
synced_at: 2026-01-10T02:06:24Z
```

# BUG: false positive in `df.attrs.update()`

---

_Issue opened by @janosh on 2025-08-19 06:12_

### Summary

```python
# ty-pandas-attrs.py
import pandas as pd

df = pd.DataFrame({"A": [1, 2, 3], "B": [4, 5, 6]})
df.attrs.update(name="dummy", foo="bar") # <-- type error here
```

`ty 0.0.1-alpha.18 (d697cc092 2025-08-14)` raises a type error:

> Argument to bound method `update` is incorrect: Expected `Mapping[str, Any]`, found `dict[Hashable, Any]`tyinvalid-argument-type


### Expected Behavior

changing to

```diff
- df.attrs.update(name="dummy", foo="bar")
+ df.attrs.update({"name": "dummy", "foo": "bar"})
```

resolves the type error. both work at run time and i'd expect both to type check fine

first reported in https://github.com/pandas-dev/pandas/issues/62140

### Version

ty 0.0.1-alpha.18 (d697cc092 2025-08-14)

---

_Comment by @carljm on 2025-08-19 16:43_

Thank your for the (very nicely minimized) report!

Pandas [types `DataFrame::attrs` as `dict[Hashable, Any]`](https://github.com/pandas-dev/pandas/blob/main/pandas/core/generic.py#L317). The typeshed definition for `MutableMapping::update` (inherited by `dict`) has [one overload](https://github.com/python/typeshed/blob/main/stdlib/typing.pyi#L810) that accepts `**kwargs`, and that overload requires that `self` be of type `Mapping[str, _VT]` -- that is, its keys must be strings. A `dict` with key type `Hashable` is not assignable to a `Mapping` with key type `str`, so this overload fails to match the call.

It feels like the type of `self` on that `**kwargs` overload for `update` could be less strict somehow, but it's not clear exactly how. It does need to be a mapping for which strings are valid keys; it doesn't need to be a mapping whose keys are _all_ strings. The underlying issue here is that in this particular case we'd prefer the key type to behave contravariantly, but dictionaries (and mutable mappings generally) are invariant in their key type (which is required for their soundness generally, but not in a way that impacts this particular example.)

So ultimately I think this type error is correct, given the type system specification and the type information currently provided by pandas and the standard library. (Of course, it can still be a false positive relative to runtime behavior, but the fix for that will need to be in the type spec, or in typeshed, or in pandas / pandas-stubs, not just in ty.) Mypy and pyright both also give versions of exactly the same error on the same code snippet.

Off the top of my head I am not sure exactly what would be a good fix here; it may be something worth raising as a question in the Typing section of discuss.python.org. If in practice dataframe attrs all have string keys, then tightening the type annotation on `DataFrame::attrs` to `dict[str, Any]` would be the simplest fix.

Closing this as I don't think this is actionable for ty, but we will certainly update if there's a broader consensus of type checkers that this case should be handled differently somehow.

Thanks again for the great report!

---

_Closed by @carljm on 2025-08-19 16:43_

---

_Comment by @AlexWaygood on 2025-08-19 16:53_

> It feels like the type of `self` on that `**kwargs` overload for `update` could be less strict somehow, but it's not clear exactly how. It does need to be a mapping for which strings are valid keys; it doesn't need to be a mapping whose keys are _all_ strings.

hmmm, this patch makes our error go away? I can see about making a typeshed PR

```diff
diff --git a/crates/ty_vendored/vendor/typeshed/stdlib/typing.pyi b/crates/ty_vendored/vendor/typeshed/stdlib/typing.pyi
index 90743a7cc3..f926428414 100644
--- a/crates/ty_vendored/vendor/typeshed/stdlib/typing.pyi
+++ b/crates/ty_vendored/vendor/typeshed/stdlib/typing.pyi
@@ -1319,13 +1319,13 @@ class MutableMapping(Mapping[_KT, _VT]):
         """
 
     @overload
-    def update(self: Mapping[str, _VT], m: SupportsKeysAndGetItem[str, _VT], /, **kwargs: _VT) -> None: ...
+    def update(self: SupportsKeysAndGetItem[str, _VT], m: SupportsKeysAndGetItem[str, _VT], /, **kwargs: _VT) -> None: ...
     @overload
     def update(self, m: Iterable[tuple[_KT, _VT]], /) -> None: ...
     @overload
-    def update(self: Mapping[str, _VT], m: Iterable[tuple[str, _VT]], /, **kwargs: _VT) -> None: ...
+    def update(self: SupportsKeysAndGetItem[str, _VT], m: Iterable[tuple[str, _VT]], /, **kwargs: _VT) -> None: ...
     @overload
-    def update(self: Mapping[str, _VT], **kwargs: _VT) -> None: ...
+    def update(self: SupportsKeysAndGetItem[str, _VT], **kwargs: _VT) -> None: ...
 
 Text = str
```

---

_Comment by @AlexWaygood on 2025-08-19 17:17_

hmm, it's not quite so simple. The ty error only goes away if I make that change because we're missing some protocol features; mypy still doesn't like the code in the original snippet even with that change to typeshed. The reason is that even `SupportsKeysAndGetItem` is invariant in its first type parameter. ~~(I'm not totally sure it needs to be, though?)~~ Edit, yeah, it obviously does need to be.

---

_Comment by @AlexWaygood on 2025-08-19 17:23_

_this_ seems to make even mypy happy with the original snippet, however: at last, a protocol with a contravariant parameter for the key type!

```diff
diff --git a/crates/ty_vendored/vendor/typeshed/stdlib/typing.pyi b/crates/ty_vendored/vendor/typeshed/stdlib/typing.pyi
index 90743a7cc3..10ed9dc525 100644
--- a/crates/ty_vendored/vendor/typeshed/stdlib/typing.pyi
+++ b/crates/ty_vendored/vendor/typeshed/stdlib/typing.pyi
@@ -25,7 +25,7 @@ import collections  # noqa: F401  # pyright: ignore[reportUnusedImport]
 import sys
 import typing_extensions
 from _collections_abc import dict_items, dict_keys, dict_values
-from _typeshed import IdentityFunction, ReadableBuffer, SupportsKeysAndGetItem
+from _typeshed import IdentityFunction, ReadableBuffer, SupportsGetItem, SupportsKeysAndGetItem
 from abc import ABCMeta, abstractmethod
 from re import Match as Match, Pattern as Pattern
 from types import (
@@ -1319,13 +1319,13 @@ class MutableMapping(Mapping[_KT, _VT]):
         """
 
     @overload
-    def update(self: Mapping[str, _VT], m: SupportsKeysAndGetItem[str, _VT], /, **kwargs: _VT) -> None: ...
+    def update(self: SupportsGetItem[str, _VT], m: SupportsKeysAndGetItem[str, _VT], /, **kwargs: _VT) -> None: ...
     @overload
     def update(self, m: Iterable[tuple[_KT, _VT]], /) -> None: ...
     @overload
-    def update(self: Mapping[str, _VT], m: Iterable[tuple[str, _VT]], /, **kwargs: _VT) -> None: ...
+    def update(self: SupportsGetItem[str, _VT], m: Iterable[tuple[str, _VT]], /, **kwargs: _VT) -> None: ...
     @overload
-    def update(self: Mapping[str, _VT], **kwargs: _VT) -> None: ...
+    def update(self: SupportsGetItem[str, _VT], **kwargs: _VT) -> None: ...
 
 Text = str
```

---

_Comment by @AlexWaygood on 2025-08-19 17:45_

The stubs were actually changed very recently in typeshed here (I merged the change during pycon only a few months ago!) https://github.com/python/typeshed/pull/14099

---

_Comment by @AlexWaygood on 2025-08-19 17:52_

trying out a typeshed change here: https://github.com/python/typeshed/pull/14593

---

_Comment by @AlexWaygood on 2025-08-19 19:07_

> trying out a typeshed change here: [python/typeshed#14593](https://github.com/python/typeshed/pull/14593)

Now merged -- thanks for the report @janosh! We'll pick up the change in our next automated typeshed sync.

---

_Comment by @janosh on 2025-08-20 12:55_

@AlexWaygood wonderful, thanks for the quick fix! really enjoying `ty`. i've been hoping astral does a type checker for well over a year and it definitely delivers :)

---
