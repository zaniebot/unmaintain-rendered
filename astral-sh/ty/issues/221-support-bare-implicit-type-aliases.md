```yaml
number: 221
title: support bare/implicit type aliases
type: issue
state: closed
author: carljm
labels:
  - typing semantics
assignees: []
created_at: 2025-01-09T18:56:57Z
updated_at: 2025-12-03T09:42:13Z
url: https://github.com/astral-sh/ty/issues/221
synced_at: 2026-01-12T15:54:22Z
```

# support bare/implicit type aliases

---

_@carljm_

We already support simple cases of bare type aliases (e.g. `MyInt = int`) if they fall out correctly from evaluating the RHS as a value expression. But for more complex (and useful!) cases (e.g. `MyType = int | str`) we fail because we evaluate the RHS as a value expression and it needs to be evaluated as a type expression.

This requires some heuristics to decide what assignments should be treated as type aliases. (We should explore what other type checkers do here.)

It will also require a decision about how to handle the use of something we have decided is a type alias, in a value expression. Is this forbidden? Or do we need to have two types for that particular name, one value-expression type and one type-expression type?

---

_Comment by @sharkdp on 2025-01-09 20:00_

Just for reference, we have existing support for non-generic PEP 695 type aliases: [mdtests](https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/resources/mdtest/pep695_type_aliases.md)

---

_Comment by @MichaReiser on 2025-03-22 11:59_

This might also require changes to the LSP (Goto type definition)

---

_Renamed from "[red-knot] support type aliases (PEP 695, typing.TypeAlias, and bare/implicit)" to "support type aliases (PEP 695, typing.TypeAlias, and bare/implicit)" by @MichaReiser on 2025-05-07 15:27_

---

_Comment by @carljm on 2025-05-08 15:38_

Here's a common implicit type alias case (from #275): https://play.ty.dev/1fa1c80b-f019-4d7d-8e59-80ad8e5fb2c4

```py
from typing import Literal

HandleDupes = Literal["keep-first", "keep-last", "mean", None]

def test(handle_dup_idx: HandleDupes = None):
    if handle_dup_idx in ("keep-first", "keep-last"):
        # get possibly-unbound-attribute on next line
        reveal_type(handle_dup_idx)
    return None
```

We should know that the revealed type is `Literal["keep-first", "keep-last"]`, and we do if a PEP 695 type alias is used, or if the `HandleDupes` type is inlined into the parameter annotation, but we don't understand the implicit type alias.

---

_Label `typing semantics` added by @AlexWaygood on 2025-05-11 07:48_

---

_Renamed from "support type aliases (PEP 695, typing.TypeAlias, and bare/implicit)" to "support bare/implicit type aliases" by @carljm on 2025-05-30 00:53_

---

_Comment by @carljm on 2025-05-30 00:54_

Repurposing this issue to cover just bare/implicit type aliases, and added https://github.com/astral-sh/ty/issues/544 for `typing.TypeAlias`. We already have pretty good support for PEP 695 type aliases (modulo needing better support for recursive aliases.)

---

_Assigned to @carljm by @carljm on 2025-08-15 14:47_

---

_Comment by @andythomas on 2025-08-26 20:19_

I am not sure if this is also a duplicate as the linked #1025 

```python
import numpy

a = [3.2, 4.3]
b = numpy.int8(a)[0]
print(b)
```

It correctly prints `3` and `ty` says:

```
error[non-subscriptable]: Cannot subscript object of type `signedinteger[_8Bit]` with no `__getitem__` method
 --> test_np.py:4:5
  |
3 | a = [3.2, 4.3]
4 | b = numpy.int8(a)[0]
  |     ^^^^^^^^^^^^^
  |
info: rule `non-subscriptable` is enabled by default
```



---

_Unassigned @carljm by @MichaReiser on 2025-10-17 14:46_

---

_Assigned to @sharkdp by @MichaReiser on 2025-10-17 15:27_

---

_Comment by @sharkdp on 2025-11-03 21:06_

(Possibly incomplete) list of things that we need to support

- [x] PEP 604 unions
- [x] `Literal`
- [x] `LiteralString` / `NoReturn` / `Never`
- [x] `Annotated`
- [x] `Optional`
- [x] `Tuple`
- [x] `Union`
- [x] `type[…]`, `Type[…]`
- [x] `List`, `Dict`, `Set`, `FrozenSet`, `ChainMap`, `Counter`, `DefaultDict`, `Deque`, `OrderedDict`
- [x] `Callable[…]`
- [x] Explicit specialization of implicit type aliases that are generic


---

_Comment by @carljm on 2025-11-21 15:09_

https://github.com/astral-sh/ty/issues/573 and https://github.com/astral-sh/ty/issues/1429 are bugs that were closed as duplicates of https://github.com/astral-sh/ty/issues/544 (TypeAlias support) but don't yet work correctly with that fix landed. I suspect they require the generic type alias support, so we should check them again once that lands.

---

_Closed by @sharkdp on 2025-11-28 19:38_

---

_Comment by @sharkdp on 2025-11-28 19:40_

We now have reasonable support for implicit type aliases, including generic ones. There are a few remaining open points (e.g. usage of *unspecialized* generic implicit type aliases in type annotations, self-referential generic implicit type aliases). I will open a new ticket listing them next week.

---

_Comment by @sharkdp on 2025-11-28 19:41_

> https://github.com/astral-sh/ty/issues/573 and https://github.com/astral-sh/ty/issues/1429 are bugs that were closed as duplicates of https://github.com/astral-sh/ty/issues/544 (TypeAlias support) but don't yet work correctly with that fix landed. I suspect they require the generic type alias support, so we should check them again once that lands.

* #573 will be resolved by https://github.com/astral-sh/ruff/pull/21553
* #1429 will *not* be resolved by https://github.com/astral-sh/ruff/pull/21553. It looks like that will also require support for generic protocols in the constraint solver. I will reopen it.

---

_Comment by @sharkdp on 2025-12-03 09:42_

> There are a few remaining open points (e.g. usage of _unspecialized_ generic implicit type aliases in type annotations, self-referential generic implicit type aliases).

The first point has already been addressed in https://github.com/astral-sh/ruff/pull/21765. I further opened these two tickets to track the remaining open issues around generic implicit/PEP613 type aliases:

* https://github.com/astral-sh/ty/issues/1739
* https://github.com/astral-sh/ty/issues/1738

---
