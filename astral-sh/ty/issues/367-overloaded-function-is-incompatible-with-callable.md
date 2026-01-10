```yaml
number: 367
title: Overloaded function is incompatible with Callable annotation that matches an overload
type: issue
state: closed
author: effigies
labels: []
assignees: []
created_at: 2025-05-13T18:51:44Z
updated_at: 2025-05-13T19:12:21Z
url: https://github.com/astral-sh/ty/issues/367
synced_at: 2026-01-10T02:34:09Z
```

# Overloaded function is incompatible with Callable annotation that matches an overload

---

_Issue opened by @effigies on 2025-05-13 18:51_

### Summary

`operator.getitem` is [defined in typeshed](https://github.com/python/typeshed/blob/64875ee375ac2aeb440e12eb2cb557e939463c0a/stdlib/_operator.pyi#L81-L84):

```python
@overload
def getitem(a: Sequence[_T], b: slice, /) -> Sequence[_T]: ...
@overload
def getitem(a: SupportsGetItem[_K, _V], b: _K, /) -> _V: ...
```

Attempting to assign it to a variable with type `ty.Callable[[ty.Mapping[K, V], K], V]` fails with:

```
Object of type `Overload[(a: Sequence[_T], b: slice[Any, Any, Any], /) -> Sequence[_T], (a: SupportsGetItem[_K, _V], b: _K, /) -> _V]` is not assignable to `((Mapping[K, V], K, /) -> V) | None` (invalid-assignment)
```

Creating my own getitem succeeds:

```py
def mygetitem(a: SupportsGetItem[K, V], b: K, /) -> V:
    return a[b]
```

I'm not sure I have the type theory language to describe the situation, but you should be able to define a function that overloads to a broader set of inputs to a variable that accepts a narrower set of inputs. Mypy is okay with this code. (Pyright is okay with it, as long as `getterfunc` is a parameter to a function, not a standalone variable.)

https://play.ty.dev/066b782e-0719-46a3-b69f-e2f0aff47190

### Version

ty 0.0.1-alpha.1

---

_Comment by @carljm on 2025-05-13 19:12_

Thanks for the report! The root cause of this is #95 . Will close as duplicate, but it's useful to add this example.

---

_Closed by @carljm on 2025-05-13 19:12_

---
