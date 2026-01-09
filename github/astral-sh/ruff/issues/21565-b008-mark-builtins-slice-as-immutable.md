---
number: 21565
title: "B008: mark builtins.slice() as immutable"
type: issue
state: closed
author: anabelle2001
labels:
  - bug
assignees: []
created_at: 2025-11-21T17:49:27Z
updated_at: 2025-12-08T19:00:44Z
url: https://github.com/astral-sh/ruff/issues/21565
synced_at: 2026-01-07T13:12:16-06:00
---

# B008: mark builtins.slice() as immutable

---

_Issue opened by @anabelle2001 on 2025-11-21 17:49_

# Example
```py
def a(b=float("inf")): ... # True Negative
def c(d=slice(None)): ... # False Positive, as slice() is immutable
```

There is also the semi-related corner case of `def e(f: slice = dict()): ...`. This is clearly erroneous code, but it is incorrectly flagged as violating B008, despite the docs saying "Parameters with immutable type annotations will be ignored by this rule". (e.g., `def g(h: int = dict()): ...`, does not trigger B008)

see also: [docs.python.org > builtins > slice](https://docs.python.org/3/library/functions.html#slice)

### Version

_No response_

---

_Comment by @ntBre on 2025-11-21 21:15_

Thanks! It makes sense to me to update this list to include slice:

https://github.com/astral-sh/ruff/blob/f04a18d8f82afc8d60af053d1b121f32d719b454/crates/ruff_python_stdlib/src/typing.rs#L329

based on the linked docs:

> Slice objects have read-only data attributes start, stop, and step which merely return the argument values (or their default). They have no other explicit functionality

I think that should also resolve the annotation case.

---

_Label `rule` added by @ntBre on 2025-11-21 21:15_

---

_Label `rule` removed by @ntBre on 2025-11-21 21:15_

---

_Label `bug` added by @ntBre on 2025-11-21 21:15_

---

_Referenced in [astral-sh/ruff#21823](../../astral-sh/ruff/pulls/21823.md) on 2025-12-06 13:06_

---

_Closed by @ntBre on 2025-12-08 19:00_

---
