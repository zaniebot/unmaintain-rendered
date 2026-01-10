```yaml
number: 955
title: lazy snapshot sweeping needs to invalidate constraints in nested scopes
type: issue
state: closed
author: oconnor663
labels:
  - bug
  - narrowing
assignees: []
created_at: 2025-08-07T20:49:35Z
updated_at: 2025-08-19T12:51:28Z
url: https://github.com/astral-sh/ty/issues/955
synced_at: 2026-01-10T02:06:24Z
```

# lazy snapshot sweeping needs to invalidate constraints in nested scopes

---

_Issue opened by @oconnor663 on 2025-08-07 20:49_

### Summary

Here's an inconsistent inferred type on `main` today:

```py
def f():
    x = None
    def g():
        if x is None:
            def h():
                # Reveals `None` but is "foo" at runtime.
                reveal_type(x)
            return h
    h = g()
    x = "foo"
    h()
```

My understanding is that the reassignment `x = "foo"` invalidates shapshots of `x` in `f`, but it doesn't invalidate the snapshot of `x` in `g` that captures the `x is None` constraint.

Here's a second variant of this issue, where we don't have any functions-returning-functions, but we do have some `nonlocal` bindings:

```py
def f():
    x = None
    def g():
        if x is None:
            def h1():
                # Reveals `None` but is "foo" at runtime.
                reveal_type(x)
            def h2():
                nonlocal x
                x = "foo"
            h2()
            h1()
    g()
```

That second version might fall under the umbrella of "we don't currently infer nested bindings" though? I'm not sure. cc @carljm @mtshiba

### Version

_No response_

---

_Label `bug` added by @carljm on 2025-08-07 22:22_

---

_Label `narrowing` added by @carljm on 2025-08-07 22:22_

---

_Comment by @carljm on 2025-08-08 17:11_

@mtshiba is this something you could look into?

---

_Comment by @mtshiba on 2025-08-11 01:49_

Yes, I'll work to fix this.

---

_Closed by @carljm on 2025-08-15 00:52_

---
