```yaml
number: 16994
title: "[red-knot] support named tuples and typing.NamedTuple"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2025-03-26T18:09:08Z
updated_at: 2025-04-30T20:57:15Z
url: https://github.com/astral-sh/ruff/issues/16994
synced_at: 2026-01-12T15:54:55Z
```

# [red-knot] support named tuples and typing.NamedTuple

---

_@carljm_

_No description provided._

---

_Label `red-knot` added by @carljm on 2025-03-26 18:09_

---

_Added to milestone `Red Knot GA` by @carljm on 2025-03-27 19:11_

---

_Comment by @johnslavik on 2025-03-31 07:58_

`super()` is unsupported in `typing.NamedTuple`s--would be cool to also catch that (https://github.com/python/cpython/pull/130082)

---

_Comment by @dcreager on 2025-04-28 19:31_

https://github.com/astral-sh/ruff/pull/16538 currently adds a bunch of new `NamedTuple`-related false positives.

`NamedTuple` can be used either as a base class or as a function:

```py
class A(NamedTuple):
    x: int
    y: str

A = NamedTuple("A", [("x", int), ("y", str)])
```

typeshed supports the latter by adding some `__init__` overloads to `NamedTuple`:

```py
@overload
def __init__(self, typename: str, fields: Iterable[tuple[str, Any]], /) -> None: ...

@overload
def __init__(self, typename: str, fields: None = None, /, **kwargs: Any) -> None: ...
```

For the former, we'll probably need to reuse some of the `dataclass` machinery to create a custom `__init__` method for each `NamedTuple` subclass.

---

_Comment by @carljm on 2025-04-28 22:35_

We can also temporarily eliminate all these false positives with a simple special-case handling for instantiating a named tuple that accepts any arguments; we had a similar special-case in place for dataclass before we handled its constructor properly. (Though probably doing this correctly for NamedTuple by emulating our existing dataclass support wouldn't be too hard?)

---

_Removed from milestone `Red Knot GA` by @carljm on 2025-04-28 22:35_

---

_Added to milestone `Red Knot Alpha` by @carljm on 2025-04-28 22:35_

---

_Comment by @carljm on 2025-04-28 22:36_

Marking this for alpha simply due to the quantity of false positives it causes in the ecosystem; the alpha "fix" can be simply the special case to silence all the false positives for now.

---

_Comment by @johnslavik on 2025-04-29 08:25_

I know this isn't forced by standard, but I'm also advocating for [namedtuples to be final](https://github.com/bswck/make-typed-namedtuples-final). You may or may not want to check that. Or disrecommend that with a linter, if that makes sense. I'm planning to make it into a PEP someday.

---

_Assigned to @sharkdp by @sharkdp on 2025-04-30 08:22_

---

_Comment by @sharkdp on 2025-04-30 20:57_

Initial support for `NamedTuple` was merged in #17738. I'm closing this for now. There are some open TODOs in the test suite and some ideas in the issue here, but I'm unsure if it's worth opening a new ticket just yet. I think we'll automatically be forced to implement more functionality eventually, either by bug reports, ecosystem false positives or when we add support for running the typing conformance checks.

---

_Closed by @sharkdp on 2025-04-30 20:57_

---
