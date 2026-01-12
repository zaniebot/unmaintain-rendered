```yaml
number: 710
title: Narrow nonlocal type based on position of inner scope
type: issue
state: closed
author: sharkdp
labels:
  - control flow
assignees: []
created_at: 2025-06-26T11:45:22Z
updated_at: 2025-11-14T15:25:29Z
url: https://github.com/astral-sh/ty/issues/710
synced_at: 2026-01-12T15:54:23Z
```

# Narrow nonlocal type based on position of inner scope

---

_@sharkdp_

With https://github.com/astral-sh/ruff/pull/18750 merged, we now consider all reachable bindings for nonlocal types. This can cause overly wide union types in cases where the inner scope is only executed after narrowing the type in the outer scope through an assignment. For example:

```py
def outer(x=None):
    # …

    x = 1

    def inner():
        reveal_type(x)  # Unknown | None | Literal[1], should not contain `None`
    
    inner()
```

This should be fixed by considering the position of the inner scope within the enclosing scope. Only bindings that appear after the definition of the inner scope should be considered.

A similar problem appears with type narrowing through conditions.

```py
def outer(x: int | None):
    if x is not None:
        def inner():
            reveal_type(x)  # int | None, should be int
        
        inner()
```

Fixing this as well might follow a similar strategy, if we implement the "treat every new narrowing of a definition as a new definition" strategy proposed in https://github.com/astral-sh/ty/issues/690.

---

_Label `control flow` added by @sharkdp on 2025-06-26 11:45_

---

_Comment by @carljm on 2025-11-14 15:25_

This is now fixed, at least for both examples given here.

---

_Closed by @carljm on 2025-11-14 15:25_

---
