```yaml
number: 129
title: Add subtyping / assignability between callable types and classes
type: issue
state: closed
author: dhruvmanila
labels:
  - help wanted
  - type properties
assignees: []
created_at: 2025-04-10T21:46:44Z
updated_at: 2025-07-04T02:13:06Z
url: https://github.com/astral-sh/ty/issues/129
synced_at: 2026-01-12T15:54:22Z
```

# Add subtyping / assignability between callable types and classes

---

_@dhruvmanila_

> Some other false positives appear to be because we lack handling of assignability/subtyping of a class literal or subclass-of type to a `Callable` type. I think the proper handling here (barring metaclass `__call__` handling) would consider the class to be an intersection of its `__init__` and `__new__` signatures. But we could also do a temporary version that just uses the current `Type::signatures()` implementation as forgiving gradual callable.
>
> _Originally posted in https://github.com/astral-sh/ruff/pull/16512#issuecomment-2784668982_

This involves adding subtyping and assignability between:
- [x] `CallableType` and `Type::ClassLiteral` (high priority)
- [x] `CallableType` and `Type::Instance` (class instance with `__call__` method)
- [x] `CallableType` and `Type::SubclassOf`
- [x] `CallableType` and `Type::GenericAlias`

---

_Comment by @carljm on 2025-04-11 03:19_

Subtyping/assignability between `CallableType` and `SubclassOf` is unsound (because no Liskov enforcement on constructors), but we probably should do it anyway. I'd at least like a TODO comment to note that it's unsound so in future we can consider an opt-in option to disable it.

---

_Comment by @dhruvmanila on 2025-04-17 14:52_

This seems like a pretty high priority because it shows up a lot in the ecosystem changes in astral-sh/ruff#17366, specifically between callable types and class literals.

---

_Label `help wanted` added by @dhruvmanila on 2025-04-18 12:56_

---

_Comment by @sharkdp on 2025-04-23 15:03_

Another thing that would solve a lot of false positives would be assignability of instances of classes with a `__call__` method to a `Callable` type. For example, I see a lot of false positives on usages of `functools.partial`:
```
from typing import Callable
from functools import partial

def f(x: int, y: str) -> None: ...

c: Callable[[int], None] = partial(f, y="a")
```
(https://types.ruff.rs/9f32b840-e3bd-42dd-8dea-14136e1eb6c4)

---

_Comment by @dhruvmanila on 2025-04-23 15:12_

> Another thing that would solve a lot of false positives would be assignability of instances of classes with a `__call__` method to a `Callable` type.

I think we have that but it's only for `is_subtype_of`, we would need to add it to `is_assignable_to` as well.

https://github.com/astral-sh/ruff/blob/e7f38fe74ba642cbd4cf286d8524f4172cbe30e2/crates/red_knot_python_semantic/src/types.rs#L1231-L1239

This seems a bit unfortunate that we need to duplicate this in both `is_subtype_of` and `is_assignable_to` because the match arm in `is_subtype_of` would only work for fully static type and `partial`'s `__call__` method is gradual:

https://github.com/python/typeshed/blob/87f599dc8312ac67b941b5f2b47274534a1a2d3a/stdlib/functools.pyi#L118-L127

So, this seems like an easy fix?

---

_Comment by @sharkdp on 2025-04-23 17:19_

Okay, thank you for the references, I can look into it.

---

_Comment by @carljm on 2025-04-23 18:09_

This is another point in favor of the idea that we should collapse `is_subtype_of` and `is_assignable_to` into a single implementation with a strategy parameter.

---

_Comment by @dhruvmanila on 2025-05-02 14:11_

Progress on assignability between `Callable` and `ClassLiteral`:
* https://github.com/astral-sh/ruff/pull/17469
* https://github.com/astral-sh/ruff/pull/17533
* https://github.com/astral-sh/ruff/pull/17638

---

_Comment by @sharkdp on 2025-05-03 18:41_

There's also:
* https://github.com/astral-sh/ruff/pull/17704

---

_Renamed from "[red-knot] Add subtyping / assignability between callable types and classes" to "Add subtyping / assignability between callable types and classes" by @MichaReiser on 2025-05-07 15:25_

---

_Label `subtyping/assignability` added by @AlexWaygood on 2025-05-10 18:07_

---

_Comment by @MatthewMckee4 on 2025-07-03 18:09_

I also think we have support for `CallableType` and `Type::GenericAlias`

---

_Comment by @carljm on 2025-07-04 02:13_

Confirmed! https://play.ty.dev/3349c0d7-d7c3-42a1-8f80-5dbbcabf1762

---

_Closed by @carljm on 2025-07-04 02:13_

---
