---
number: 16507
title: "Prevent non-new-variable `for` loop targets"
type: issue
state: open
author: InSyncWithFoo
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-03-04T18:33:35Z
updated_at: 2025-03-05T07:48:49Z
url: https://github.com/astral-sh/ruff/issues/16507
synced_at: 2026-01-07T13:12:16-06:00
---

# Prevent non-new-variable `for` loop targets

---

_Issue opened by @InSyncWithFoo on 2025-03-04 18:33_

The `target` of a `for` loop can be names (`global`, `nonlocal`, local), subcripts, attributes and/or any derivation of those:

```python
a: str
b = 42
global c
nonlocal d

# Valid syntax
for a, (b, c, [d, e[f], g.h]), i.j().k, l in m: ...
```

Only new variables should be allowed as loop targets. That is, in the above example, only `a` and `l` would be allowed: `l` is defined by the loop itself, while `a` doesn't have a value prior to the loop.

A fix for this is trivial:

```python
# Before
for a, (b, c, [d, e[f], g.h]), i.j().k, l in m: ...

# After
for _element in l:
    a, (b, c, [d, e[f], g.h]), i.j().k, l = _element
    ...
```

This issue is a follow-up to #16496. See that issue for a few edge cases related to the fix.


---

_Label `rule` added by @ntBre on 2025-03-04 18:35_

---

_Label `needs-decision` added by @ntBre on 2025-03-04 18:35_

---

_Comment by @MichaReiser on 2025-03-05 07:48_

Thanks for opening this issue and the detailed write up! 

I think the rule overall does make sense to me but I'm not sure if this pattern is common enough to justify adding a rule

---
