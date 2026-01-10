```yaml
number: 2297
title: "\"incorrect\" inlay hint on attribute assignment"
type: issue
state: closed
author: MatthewMckee4
labels:
  - server
  - attribute access
assignees: []
created_at: 2026-01-01T19:16:36Z
updated_at: 2026-01-05T08:19:13Z
url: https://github.com/astral-sh/ty/issues/2297
synced_at: 2026-01-10T01:56:41Z
```

# "incorrect" inlay hint on attribute assignment

---

_Issue opened by @MatthewMckee4 on 2026-01-01 19:16_

### Summary

In the following example we see the inlay hint `: int`, but in my opinion this is not very useful because the actual type of `self.x` is `Unknown | int`.

```py
class Foo:
    def __init__(self, x: int):
        self.x = x
```

I feel like we should infer the type of the lhs here instead of the rhs.

I have a initial working solution, that works semi well.

---

_Renamed from ""incorrect" inlay hint" to ""incorrect" inlay hint on attribute assignment" by @MatthewMckee4 on 2026-01-01 19:16_

---

_Comment by @mtshiba on 2026-01-02 02:51_

Outside the method, we cannot definitively infer the type of `Foo.x` to be `int`, but we can at least infer that it is `int` within `__init__`. This can be considered a kind of narrowing.

```python
class Foo:
    def __init__(self, x: int):
        self.x = x
        reveal_type(self.x)  # int
        self.x = 1
        reveal_type(self.x)  # Literal[1]

reveal_type(Foo(1).x)  # Unknown | int
```

So the issue becomes whether, when displaying attribute types, we should display the global type or the local type. I think the current approach (local) is fine.

---

_Label `attribute access` added by @mtshiba on 2026-01-02 02:51_

---

_Comment by @mtshiba on 2026-01-03 01:49_

However, it can indeed feel inconvenient that when examining the type of an implicit instance attribute, we cannot determine the global type from its usage within a method. It might be helpful to display the global type as well, as supplementary information alongside the type shown on hover.

---

_Label `server` added by @mtshiba on 2026-01-03 01:49_

---

_Comment by @MatthewMckee4 on 2026-01-03 04:30_

I agree that we should show some other information. I that recently, quite a few issues people have had on discord, make it seem like there is a bug in ty, when in fact one of their instance attributes are unioned with unknown, but it's hard to see that when we don't show that information, and rather we show something that seems wrong.

---

_Comment by @MichaReiser on 2026-01-03 17:37_

I agree that this is confusing. It also means that baking an inlay can create new issues downstreams

---

_Added to milestone `Stable` by @MichaReiser on 2026-01-03 17:38_

---

_Removed from milestone `Stable` by @MichaReiser on 2026-01-03 17:38_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2026-01-03 17:38_

---

_Comment by @carljm on 2026-01-04 22:59_

I don't think that we should invest time/complexity in addressing this on the inlay-hint side of things, because I think we should stop unioning attribute types with `Unknown` in these cases by default. The confusion is caused by the Unknown unioning, not by anything specific to inlay hints.

> It also means that baking an inlay can create new issues downstreams

This is true in many situations, and it will always be true; it's just the nature of baking inlays, it's not at all specific to attributes or the `Unknown` unioning. The only reason this snippet is more than two lines is because we suppress "trivial" inlay hints:

```py
def returns_int() -> int:
    return 1

x = returns_int()
x = "foo"
```

This code is fine as-is, but if you bake the inlay hint of `int` on the first assignment to `x`, the second assignment becomes a type error.

A type annotation is a declared upper bound on the type of that variable, the inlay hint is just the inferred type of the RHS. So baking inlay hints is, by nature, something that changes the semantics of the code and may cause new errors.

---

_Comment by @carljm on 2026-01-04 23:00_

(I would suggest to close this as duplicate of #1240, personally.)

---

_Comment by @MichaReiser on 2026-01-05 08:19_

> (I would suggest to close this as duplicate of https://github.com/astral-sh/ty/issues/1240, personally.)

I'm fine with this as long as the conclusion on #1240 is that this isn't behind an option and is prioritized before stable (which both seem the case today)

---

_Closed by @MichaReiser on 2026-01-05 08:19_

---
