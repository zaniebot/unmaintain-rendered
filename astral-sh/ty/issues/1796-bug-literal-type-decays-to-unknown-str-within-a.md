```yaml
number: 1796
title: "BUG: `Literal` type decays to `Unknown | str` within a listcomp"
type: issue
state: closed
author: neutrinoceros
labels: []
assignees: []
created_at: 2025-12-07T17:15:53Z
updated_at: 2025-12-07T19:12:19Z
url: https://github.com/astral-sh/ty/issues/1796
synced_at: 2026-01-12T15:54:25Z
```

# BUG: `Literal` type decays to `Unknown | str` within a listcomp

---

_@neutrinoceros_

### Summary

I made an attempt at finding an existing report for this but I think I miss the vocabulary to describe this problem in a searchable way; so apologies if this is a duplicate.

In short, here's an MRE
```py
# t.py
from typing import Literal

Fruit = Literal["banana", "kiwi"]

def eat_fruit(frt: Fruit) -> int:
    print(f"Hum... {frt}")
    return 1

def process(frt: Fruit, other: Fruit):
    a, b = [eat_fruit(f) for f in [frt, other]]
```
I believe this code is perfectly type-consistent, however `ty check` does emit a diagnostic for it

```
❯ ty check t.py
error[invalid-argument-type]: Argument to function `eat_fruit` is incorrect
  --> t.py:10:23
   |
 9 | def process(frt: Fruit, other: Fruit):
10 |     a, b = [eat_fruit(f) for f in [frt, other]]
   |                       ^ Expected `Literal["banana", "kiwi"]`, found `Unknown | str`
   |
info: Element `str` of this union is not assignable to `Literal["banana", "kiwi"]`
info: Function defined here
 --> t.py:5:5
  |
3 | Fruit = Literal["banana", "kiwi"]
4 |
5 | def eat_fruit(frt: Fruit) -> int:
  |     ^^^^^^^^^ ---------- Parameter declared here
6 |     print(f"Hum... {frt}")
7 |     return 1
  |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

### Version

ty 0.0.1-alpha.32 (84a188116 2025-12-05)

---

_Renamed from "BUG: `Literal` type decays to `Unknown | str` within a listcomp" to "BUG: `Literal` type decays to `Unknown | str`" by @neutrinoceros on 2025-12-07 17:17_

---

_Renamed from "BUG: `Literal` type decays to `Unknown | str`" to "BUG: `Literal` type decays to `Unknown | str` within a listcomp" by @neutrinoceros on 2025-12-07 17:18_

---

_Comment by @AlexWaygood on 2025-12-07 18:25_

Thank you! I believe this is https://github.com/astral-sh/ty/issues/1602. We don't currently propagate type context (provided here by the parameter annotations `frt: Fruit` and `other: Fruit`) from outer scopes into inner scopes. (An inner scope is created implicitly by the list comprehension.) In order to infer the `[frt, other]` list inside the list comprehension as `list[Fruit]` rather than `list[str | Unknown]`, that's what we need to do.

---

_Closed by @AlexWaygood on 2025-12-07 18:25_

---

_Comment by @neutrinoceros on 2025-12-07 19:12_

Excellent. Will follow the discussion there. Thanks a lot !

---
