```yaml
number: 713
title: "[possibly-unbound-attribute] False positive on result from while loop"
type: issue
state: closed
author: JP-Ellis
labels:
  - bug
  - control flow
assignees: []
created_at: 2025-06-27T03:37:43Z
updated_at: 2025-06-30T11:11:54Z
url: https://github.com/astral-sh/ty/issues/713
synced_at: 2026-01-12T15:54:23Z
```

# [possibly-unbound-attribute] False positive on result from while loop

---

_@JP-Ellis_

## Summary

If a maybe-`None` variable is defined in a while loop, and the while loop exits when the variable is determined to be not `None`, ty still thinks the value may be `None` resulting in a false positive of `possibly-unbound-attribute`

## Example 1

This showcases the false positive.

```python
def f() -> str | None: ...


while True:
    if v := f():
        break
print(v.capitalize())
```

```
warning[possibly-unbound-attribute]: Attribute `capitalize` on type `str | None` is possibly unbound
 --> example.py:7:7
  |
5 |     if v := f():
6 |         break
7 | print(v.capitalize())
  |       ^^^^^^^^^^^^
  |
```

Note that this false positive is not tied to the `:=` operator. The more verbose example below produces the same error:

```python
while True:
    v = f()
    if v is not None:
        break
print(v.capitalize())
```

## Example 2

This example, while functionally equivalent, does _not_ raise a false positive.

```python
def f() -> str | None: ...


while not (v := f()):
    continue
print(v.capitalize())
```

### Version

ty 0.0.1-alpha.12 (f7446a6ee 2025-06-25)

---

_Label `control flow` added by @sharkdp on 2025-06-27 07:01_

---

_Comment by @sharkdp on 2025-06-27 07:04_

Thank you for reporting this. We do not implement cyclic control flow yet. See https://github.com/astral-sh/ty/issues/232 for the issue that keeps track of this feature.



---

_Closed by @sharkdp on 2025-06-27 07:04_

---

_Comment by @JP-Ellis on 2025-06-27 07:05_

Ah, thanks! I assumed this might be a side-effect of something yet to be implemented 👍 

---

_Comment by @carljm on 2025-06-27 14:43_

I guess specifically the issue here is that we model control flow "falling off the end" of the `while` loop, while in fact that possibility is contingent on re-checking the while loop condition?

FWIW that seems fixable without fully modeling the control flow back edges -- the bug described here doesn't actually depend on the back edge.

---

_Comment by @sharkdp on 2025-06-27 19:18_

> I guess specifically the issue here is that we model control flow "falling off the end" of the `while` loop, while in fact that possibility is contingent on re-checking the while loop condition?

Hm... really? It seems to me that only example 2 follows that description, and this we already handle fine.

But you're right. My analysis here was short-sighted The problem is actually that we don't retain the narrowing constraint in the control flow merge (another instance of https://github.com/astral-sh/ty/issues/690). Everything else works just fine:
```py
while True:
    v = f()
    if v is not None:
        break
```
Before looking at the static truthiness of the conditions, there are three ways that control flow could reach the post-loop state:
1. the first evaluation of the loop condition yields false. This has a reachability constraint of `~[True]`, so it's excluded
2. the body was executed, but a later evaluation of the loop condition yielded false. This path is also guarded by a `~[True]` reachability constraint, i.e. it's also excluded
3. the loop was exited through the `break` statement. This path has a reachability constraint of `[True] AND [v is not None]`, which evaluates to an ambiguous truthiness.

The third branch is the only on that survives after the control flow merge, but we lose the narrowing constraint of `[v is not None]` when merging it with the other two branches.

I think we might be able to fix this specific example by eagerly recording `ALWAYS_TRUE` for the expression `True`, but of course that wouldn't work with any other always-truthy condition. The real solution seems to be https://github.com/astral-sh/ty/issues/690

---

_Reopened by @sharkdp on 2025-06-27 19:18_

---

_Comment by @carljm on 2025-06-27 19:20_

Ah, that clarifies, thank you!

You reopened the issue, but I actually think your analysis suggests this should be closed as a duplicate of #690?

---

_Comment by @sharkdp on 2025-06-27 19:24_

> You reopened the issue

Yes — I thought it might be neat to try and fix this specific instance by recording `ALWAYS_TRUE`/`ALWAYS_FALSE` for `True`/`False`. That might be beneficial for performance anyways. `if True` and `if False` hopefully don't appear that often, but `while True` is a frequent pattern.

---

_Comment by @sharkdp on 2025-06-27 20:54_

> try and fix this specific instance by recording `ALWAYS_TRUE`/`ALWAYS_FALSE` for `True`/`False`

Yes, that works: https://github.com/astral-sh/ruff/pull/18998 (that was just a hacky implementation, will redo that in a cleaner way on Monday).

---

_Label `bug` added by @sharkdp on 2025-06-30 10:07_

---

_Closed by @sharkdp on 2025-06-30 11:11_

---
