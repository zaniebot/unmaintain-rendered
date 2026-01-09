---
number: 16895
title: "[red-knot] Protocol method return type is always inferred as `None`"
type: issue
state: closed
author: jorenham
labels:
  - bug
  - help wanted
  - ty
assignees: []
created_at: 2025-03-21T12:31:51Z
updated_at: 2025-03-23T17:51:11Z
url: https://github.com/astral-sh/ruff/issues/16895
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Protocol method return type is always inferred as `None`

---

_Issue opened by @jorenham on 2025-03-21 12:31_

### Summary

I'm not sure if I should call this a bug or a missing feature, but either way, red-knot is not behaving according to the typing spec here:

```py
from typing import Protocol

class CanSpam[OutT](Protocol):
    def spam(self, /) -> OutT: ...
```

which reports

```
Function can implicitly return `None`, which is not assignable to return type `OutT` (lint:invalid-return-type) [Ln 4, Col 26]
```

https://playknot.ruff.rs/5d3edee7-5070-4046-a34a-6c1b8aa2340d

I'm sure can see why I think it's not behaving as it should here. If not, then I'd be happy to explain it <sub>but be aware that that will probably involve cringy examples, awkward puns, unnecessary jargon, and a non-negligible chance at me side-tracking and bursting out into a rant about something completely unrelated</sub> 👌🏻.

### Version

_No response_

---

_Label `red-knot` added by @MichaReiser on 2025-03-21 12:49_

---

_Label `bug` added by @AlexWaygood on 2025-03-21 12:58_

---

_Label `help wanted` added by @AlexWaygood on 2025-03-21 13:00_

---

_Comment by @AlexWaygood on 2025-03-21 13:01_

Yes, we need to special-case (at least) three kinds of functions and exclude them from this check:
- Methods in classes that inherit directly from `typing.Protocol`
- Methods decorated with `@abc.abstractmethod`
- Functions or methods decorated with `@typing.overload`

---

_Comment by @jorenham on 2025-03-21 13:05_

> Yes, we need to special-case (at least) three kinds of functions and exclude them from this check:
> 
> * Methods in classes that inherit directly from `typing.Protocol`
> * Methods decorated with `@abc.abstractmethod`
> * Functions or methods decorated with `@typing.overload`

I suppose that all functions in a `*.pyi` would also fit this bill

---

_Comment by @AlexWaygood on 2025-03-21 13:09_

> I suppose that all functions in a `*.pyi` would also fit this bill

they are already excluded from the check -- there's a diagnostic emitted on the `f` function in `bar.py` here but not on the `f` function in `foo.pyi`: https://playknot.ruff.rs/06f3885f-d482-41f0-b998-9b63f89ccefc

It looks like `@overload`-decorated functions are also already excluded.

---

_Referenced in [astral-sh/ruff#16900](../../astral-sh/ruff/pulls/16900.md) on 2025-03-21 16:08_

---

_Closed by @carljm on 2025-03-23 17:51_

---

_Closed by @carljm on 2025-03-23 17:51_

---
