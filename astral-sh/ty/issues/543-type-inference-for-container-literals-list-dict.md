```yaml
number: 543
title: type inference for container literals (list, dict, set) and comprehensions
type: issue
state: closed
author: carljm
labels:
  - needs-decision
  - generics
  - bidirectional inference
assignees: []
created_at: 2025-05-29T23:55:42Z
updated_at: 2025-10-17T15:31:09Z
url: https://github.com/astral-sh/ty/issues/543
synced_at: 2026-01-12T15:54:23Z
```

# type inference for container literals (list, dict, set) and comprehensions

---

_@carljm_

We should start with a pretty dumb version of this (infer solely based on the types present at construction, maybe unioned with Unknown), which will be quick and easy to implement and be fine for a lot of cases.

We have a lot of design decisions to make about how we want to improve from there. It likely intersects with https://github.com/astral-sh/ty/issues/136 and bidirectional checking.

---

_Label `generics` added by @carljm on 2025-05-29 23:55_

---

_Label `bidirectional inference` added by @carljm on 2025-05-29 23:55_

---

_Comment by @Andrej730 on 2025-05-30 18:53_

Just came across this too with simple constructors like so.

```python
y = [1,2,3]
# list[Unknown]
reveal_type(y)
```

And some comprehensions cases:
```python
a: list[int] = [1,2,3]
b = [i for i in a]
# Revealed type: `@Todo`
reveal_type(b)

d = [i for i, x in enumerate(range(5))]
# Revealed type: `@Todo`
reveal_type(d)

f = [j for j in range(5)]
# Revealed type: `@Todo`
reveal_type(f)
```

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:40_

---

_Label `needs-decision` added by @carljm on 2025-08-18 18:59_

---

_Assigned to @ibraheemdev by @carljm on 2025-08-22 14:58_

---

_Comment by @ibraheemdev on 2025-10-17 15:04_

Container inference is now largely implemented, so I'm going to close this issue in favor pf a number of smaller open issues for limitations in our bidirectional inference solver. https://github.com/astral-sh/ty/issues/1240 is the issue for the question of how we should handle unannotated collection literals, and I opened https://github.com/astral-sh/ty/issues/1388 for comprehension inference.

---

_Closed by @ibraheemdev on 2025-10-17 15:04_

---
