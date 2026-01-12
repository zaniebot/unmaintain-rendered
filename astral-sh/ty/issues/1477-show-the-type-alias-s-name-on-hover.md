```yaml
number: 1477
title: "Show the type alias's name on hover"
type: issue
state: open
author: MichaReiser
labels:
  - help wanted
  - server
  - type aliases
assignees: []
created_at: 2025-11-04T14:19:42Z
updated_at: 2025-12-19T12:15:04Z
url: https://github.com/astral-sh/ty/issues/1477
synced_at: 2026-01-12T15:54:25Z
```

# Show the type alias's name on hover

---

_@MichaReiser_

Hovering a type alias currently reveals the aliased type. 

<img width="313" height="316" alt="Image" src="https://github.com/user-attachments/assets/c2ff2c32-6f9c-4c94-8efc-f5a6ccc74b7a" />


We should show the type alias's definition instead, similar to pylance and resolve the documentation etc from the target

<img width="885" height="408" alt="Image" src="https://github.com/user-attachments/assets/30f7865c-6464-48e3-ba17-7e289cbb6b15" />

---

_Label `help wanted` added by @MichaReiser on 2025-11-04 14:19_

---

_Label `server` added by @MichaReiser on 2025-11-04 14:19_

---

_Renamed from "Show the type aliase's name on hover" to "Show the type alias's name on hover" by @AlexWaygood on 2025-11-09 17:15_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:17_

---

_Comment by @Hugo-Polloli on 2025-11-15 12:39_

I'm going to try my hand at this over the weekend :)

---

_Comment by @Hugo-Polloli on 2025-11-16 14:49_

Actually postponing working on this, before actually implementing anything I want to confirm the exact hover behavior, esp for PEP 695 type aliases.

Right now, with Pylance on Python 3.12, I see:
```py
T = int       # hover T: `(type) T = int` + docs for int
type U = int  # hover U: `(type) U = TypeAliasType` + docs for int

class Test:
    t: T  # hover t: `(variable) t: int`
          # hover T: `(type) T = int` + docs for int

    u: U  # hover u: `(variable) u: int`
          # hover U: `(type) U = type[int]` + docs for int
```
The only consistent part is that the docs always comes from the aliased target.
Also Pylance leaks internal representation (`TypeAliasType` / `type[int]`).

However, `T = int` and `type U = int` are not the same thing at runtime (the former binds `T` directly to `int`, while the latter is a `TypeAliasType` object), so I think it’s important to hint at that, but without weighing things down by mentioning `TypeAliasType` in the hover.

What I'm thinking for ty:
```py
T = int       # hover T: `(type) T = int` + docs for int
type U = int  # hover U: `(type alias) type U = int` + docs for int


class Test:
    t: T  # hover t: `(variable) t: T (int)`
          # hover T: `(type) T = int` + docs for int

    u: U  # hover u: `(variable) u: U (int)`
          # hover U: `(type alias) type U = int` + docs for int


type Box[T] = list[T]

x: Box[int]  # hover x: `(variable) x: Box[int] (list[int])`
             # hover Box: `(type alias) type Box[T] = list[T]` + docs for list
```

* `T = int` presented as a plain type binding: `(type) T = int`.
* `type U = int` / `type Box[T] = ...` presented as first-class alias objects: `(type alias) type U = int`, etc.
* Variables always show `user written-annotation (resolved-type)`.

Does this behavior match what you’d like for `ty`?

---

_Label `type aliases` added by @AlexWaygood on 2025-12-19 12:15_

---
