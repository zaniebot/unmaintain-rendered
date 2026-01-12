```yaml
number: 1317
title: "Narrow `str` to `Literal` type via `x in [\"a\", \"b\"]` check"
type: issue
state: closed
author: lypwig
labels:
  - question
  - narrowing
assignees: []
created_at: 2025-10-07T10:22:56Z
updated_at: 2025-10-07T12:49:42Z
url: https://github.com/astral-sh/ty/issues/1317
synced_at: 2026-01-12T15:54:25Z
```

# Narrow `str` to `Literal` type via `x in ["a", "b"]` check

---

_@lypwig_

### Summary

Example:

```py
from typing import Literal

def foo1(bar: str) -> Literal["a", "b", "c"]:
    return "a" if bar else "b" # ok

def foo2(bar: str) -> Literal["a", "b", "c"]:
    return bar if bar in ["a", "b"] else "c" # error
```

> Return type does not match returned value: expected `Literal["a", "b", "c"]`, found `str` (ty invalid-return-type)

[Playground](https://play.ty.dev/85ec67d0-adaf-4de8-8cb1-b08fe0e7f06b)

### Version

ty 0.0.1-alpha.20

---

_Comment by @AlexWaygood on 2025-10-07 10:31_

Thanks for the feature request!

Currently it's a deliberate decision for us not to do this kind of type narrowing, because it's not sound in all cases. For example, in the following snippet, a type checker should understand `Sneaky` here as being a subtype of `str` (and therefore would not emit an error on it if you passed an instance of `Sneaky` into your `foo2` function). But you `bar in ["a", "b"]` check doesn't narrow an instance of `Sneaky` to `Literal["a", "b"]` -- it only works for instances of exactly `str`:

```pycon
>>> class Sneaky(str):
...     def __eq__(self, other):
...         return True
...         
>>> Sneaky("mwahaha") in ["a", "b"]
True
```

If I make two modifications to your example, we do apply type narrowing in the way you'd like:

```py
from typing import Literal, LiteralString

def foo2(bar: LiteralString) -> Literal["a", "b", "c"]:
    return bar if bar in ("a", "b") else "c"
```

The changes are to:
- Use `LiteralString` rather than `str` for the `bar` annotation (objects of type `LiteralString` are guaranteed to be instances of "exactly `str`, not subclasses of `str`, so this gets around the unsoundness issue)
- Use a tuple instead of a list. That's because we currently infer the type of the list as `list[str | Unknown]`, but we infer the type of the tuple as `tuple[Literal["a"], Literal["b"]]`, which makes it easier for us to narrow the type from an `in` check.

---

_Label `narrowing` added by @AlexWaygood on 2025-10-07 10:31_

---

_Label `question` added by @AlexWaygood on 2025-10-07 10:34_

---

_Renamed from "String should be treated as literals if compared explicitly" to "Narrow `str` to `Literal` type via `x in ["a", "b"]` check" by @AlexWaygood on 2025-10-07 10:35_

---

_Closed by @AlexWaygood on 2025-10-07 12:37_

---

_Comment by @lypwig on 2025-10-07 12:41_

Thank you for the explanation and example!

I tried some similar scenarios with a tuple and it works well (even pyright is happy, with which I was facing a similar issue).

Should I assume that a tuple is generally preferred over a list, when it's not going to be modified and only contains literals?

---

_Comment by @AlexWaygood on 2025-10-07 12:49_

In general, type checkers can often infer more precise types for tuples, yes, because of the fact that tuples are immutable (therefore covariant), and because of the fact that they are special-cased by the type system. Note that if you had a very large collection of strings, though, using a set or frozenset might be more performant than using a tuple or list, even if a type checker might not like it so much :-)

---
