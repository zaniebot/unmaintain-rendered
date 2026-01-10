```yaml
number: 370
title: "No overload of bound method `__init__` matches arguments for tempfile.TemporaryDirectory"
type: issue
state: closed
author: divaltor
labels:
  - bug
  - generics
assignees: []
created_at: 2025-05-13T21:13:03Z
updated_at: 2025-05-19T15:45:41Z
url: https://github.com/astral-sh/ty/issues/370
synced_at: 2026-01-10T02:34:09Z
```

# No overload of bound method `__init__` matches arguments for tempfile.TemporaryDirectory

---

_Issue opened by @divaltor on 2025-05-13 21:13_

### Summary

Python 3.13.3

https://play.ty.dev/2ca5385c-da70-4f1b-b4d3-e4e15b5f9c6c

```py
import tempfile

# No overload of bound method `__init__` matches argumentsty(lint:no-matching-overload)
with tempfile.TemporaryDirectory() as tmp:
    ....
```

Not sure that is intented behaviour, pyright doesn't mark it as an error.

Probably related issue - https://github.com/astral-sh/ty/issues/274

### Version

v.0.0.7a (from VS Code extension)

---

_Label `bug` added by @AlexWaygood on 2025-05-14 00:05_

---

_Comment by @carljm on 2025-05-15 03:00_

Definitely not intended behavior. Seems like the explicit annotations on `self` in the typeshed `__init__` overloads are causing us trouble somehow (though I'm not yet sure why).

---

_Renamed from "No overload of bound method for tempfile.TemporaryDirectory" to "No overload of bound method `__init__` matches arguments for tempfile.TemporaryDirectory" by @carljm on 2025-05-15 03:01_

---

_Comment by @sharkdp on 2025-05-15 11:57_

> Seems like the explicit annotations on `self` in the typeshed `__init__` overloads are causing us trouble somehow (though I'm not yet sure why).

We currently call `__init__` on the [unspecialized (or: "identity"-specialized) generic class](https://github.com/astral-sh/ruff/blob/b6b7caa0238b2f8fc455a3a6769f6ca9ae65c2af/crates/ty_python_semantic/src/types.rs#L4462-L4468). We then attempt to assign `TemporaryDirectory[AnyStr]` to `TemporaryDirectory[str]` (or `TemporaryDirectory[bytes]`) during call binding and that fails.

---

_Comment by @carljm on 2025-05-15 14:47_

@dcreager I'd be interested in your thoughts on how we should handle this. It seems fairly high priority to be able to handle these instantiations.

One note is that it looks like we should not just accept the instantiation, but also infer the type of the constructed class to match the `self` annotation of the selected overload. For example:

```py
reveal_type(TemporaryDirectory("foo"))  # revealed: TemporaryDirectory[str]
reveal_type(TemporaryDirectory(b"foo"))  # revealed: TemporaryDirectory[bytes]
```

---

_Comment by @dcreager on 2025-05-15 15:26_

This is really interesting!

> One note is that it looks like we should not just accept the instantiation, but also infer the type of the constructed class to match the `self` annotation of the selected overload.

One possibility would be to use this thought to implement a hack: if an `__init__` method has an annotation for its `self` parameter, skip inference entirely and just use that as the constructed instance type. That would certainly work for this particular case.

But it wouldn't work for something more complex:

```py
class C[T, U]:
    def __init__(self: C[int, U]) -> None: ...
```

i.e. where only _some_ of the class typevars are bound by the `self` annotation.

---

Pivoting a bit, maybe we could instead update the member lookup logic for a generic alias to skip the member (i.e., treat it as missing/unbound) if the generic alias isn't assignable to the `self` annotation. But I don't know if that would be enough to make the constructor logic work, since we'd need this to happen when the generic alias is `C[T, U]` (the identity specialization).

---

Last option, I wonder if this is just showing that we need to infer typevar mappings in both directions?  i.e. what I think is happening is that we end up looking at the pair `TemporaryDirectory[str]` (the formal annotation) and `TemporaryDirectory[AnyStr]` (the actual bound self argument, which has the identity specialization applied).  That's backwards from what we normally see, where typevars occur in the formal and are bound to types that appear in the actual.  If we do the binding inference in both directions I think we would end up correctly inferring the specialization `{AnyStr = str}` for this overload.  I'd just need to convince myself that that wouldn't introduce new false positives.

---

_Comment by @sharkdp on 2025-05-15 15:32_

> One possibility would be to use this thought to implement a hack: if an `__init__` method has an annotation for its `self` parameter, skip inference entirely and just use that as the constructed instance type. That would certainly work for this particular case.

I don't understand how that would work. Wouldn't you always pick the first overload in that case? 
```pyi
@overload
def __init__(
    self: TemporaryDirectory[str],
    suffix: str | None = None,
    prefix: str | None = None,
    dir: StrPath | None = None,
    ignore_cleanup_errors: bool = False,
    *,
    delete: bool = True,
) -> None: ...
@overload
def __init__(
    self: TemporaryDirectory[bytes],
    suffix: bytes | None = None,
    prefix: bytes | None = None,
    dir: BytesPath | None = None,
    ignore_cleanup_errors: bool = False,
    *,
    delete: bool = True,
) -> None: ...
```

Edit: or would it still work because you *also* have to match the `suffix` parameter...

---

_Comment by @carljm on 2025-05-15 20:53_

The way other type checkers seem to handle this (and I think the desired behavior of the typeshed annotation?) is that `TemporaryDirectory()` is inferred as `TemporaryDirectory[str]` (both overloads match, the first one is picked), but `TemporaryDirectory(b"foo")` is inferred as `TemporaryDirectory[bytes]` -- only the second overload matches, because of the `suffix` parameter.

I think I agree entirely with @dcreager's analysis above. The "hack" mentioned would I think work correctly for this case, but not for potentially more complex cases. Still might make sense to do for now if it's a lot easier, since this case is what we actually see in commonly used typeshed cases. Doing the type inference in both directions seems like the "full" answer.

---

_Label `generics` added by @AlexWaygood on 2025-05-18 11:54_

---

_Closed by @dcreager on 2025-05-19 15:45_

---
