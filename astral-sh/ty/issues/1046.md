```yaml
number: 1046
title: Tuple unpacking from a type alias results in wrong union type
type: issue
state: closed
author: leddy231
labels: []
assignees: []
created_at: 2025-08-18T21:07:35Z
updated_at: 2025-08-19T00:54:06Z
url: https://github.com/astral-sh/ty/issues/1046
synced_at: 2026-01-10T02:06:24Z
```

# Tuple unpacking from a type alias results in wrong union type

---

_Issue opened by @leddy231 on 2025-08-18 21:07_

### Summary

The following code snippet:
```python
type Thing = tuple[str, int]


def thing() -> Thing:
    return ("1", 2)


def foo():
    a, b = thing()
    a.lower()
```

Gives this error:
```python
warning[possibly-unbound-attribute]: Attribute `lower` on type `str | int` is possibly unbound
   |
 8 | def foo():
 9 |     a, b = thing()
10 |     a.lower()
   |     ^^^^^^^
   |
info: rule `possibly-unbound-attribute` is enabled by default
```
Because it thinks `a` is `str | int` when it should be just `str`. If the type alias is removed and the return type is explicitly `tuple[str, int]` it type checks correctly.

This seems like a regression in 0.0.1a18, it works correctly in 0.0.1a17

### Version

0.0.1a18

---

_Comment by @AlexWaygood on 2025-08-18 21:18_

Thanks for the report! I can reproduce this by running `uvx ty check` on your snippet, but not on the ty playground. The ty playground is built from the `main` branch, so I suspect that after accidentally breaking this we might have accidentally fixed it again? Not sure...

---

_Comment by @AlexWaygood on 2025-08-18 21:25_

`git bisect` indicates that the error went away again on `main` after https://github.com/astral-sh/ruff/commit/b892e4548e9b71949b1a6cb52d10ccb0c61e50d3

---

_Comment by @AlexWaygood on 2025-08-18 21:31_

And `git bisect` indicates that the error was first _introduced_ in https://github.com/astral-sh/ruff/commit/13bdba5d28521e87373b72ce2e3f99a159ede644 -- so between those two commits, we had a bug.

It's much more obvious to me why https://github.com/astral-sh/ruff/commit/13bdba5d28521e87373b72ce2e3f99a159ede644 might have added a bug here (it looks like we could be missing a branch in `Type::try_iterate_with_mode()` for `Type::TypeAlias()`?) than why https://github.com/astral-sh/ruff/commit/b892e4548e9b71949b1a6cb52d10ccb0c61e50d3 would have fixed it again 😆

---

_Comment by @carljm on 2025-08-18 23:05_

We unpack type aliases when they are added to a union or intersection, so I suspect that the latter commit causes us to pass the type through a union or intersection somewhere we didn't before.

It would still be good to add the case in `try_iterate_with_mode()`, I think; I'll see if I can make a test case that still reproduces.

---

_Assigned to @carljm by @carljm on 2025-08-18 23:05_

---

_Comment by @carljm on 2025-08-18 23:42_

Not too hard to repro if we avoid returning the alias from a function call: https://play.ty.dev/e66759b0-7d25-404c-b61d-baeae942faf3

Fix incoming.

---

_Closed by @carljm on 2025-08-19 00:54_

---
