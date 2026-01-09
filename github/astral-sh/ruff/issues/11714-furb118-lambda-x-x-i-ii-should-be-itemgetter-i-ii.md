---
number: 11714
title: "FURB118, `lambda x: x[I, ii]` should be `itemgetter((I, ii))`, instead of `itemgetter(I, ii)`"
type: issue
state: closed
author: nasyxx
labels:
  - bug
  - rule
assignees: []
created_at: 2024-06-03T04:18:56Z
updated_at: 2024-06-07T03:10:54Z
url: https://github.com/astral-sh/ruff/issues/11714
synced_at: 2026-01-07T13:12:15-06:00
---

# FURB118, `lambda x: x[I, ii]` should be `itemgetter((I, ii))`, instead of `itemgetter(I, ii)`

---

_Issue opened by @nasyxx on 2024-06-03 04:18_

In refurb, `lambda x: x[I, ii]` should be `itemgetter((I, ii))`, instead of `itemgetter(I, ii)`

```python
# xx.py
xs = [1, 2, 3]
xm = map(lambda x: x[i, ii], xs)
```

```
xx.py:3:10: FURB118 [*] Use `operator.itemgetter(i, ii)` instead of defining a lambda
```

It's a tuple.

---

_Label `bug` added by @AlexWaygood on 2024-06-03 08:22_

---

_Label `rule` added by @AlexWaygood on 2024-06-03 08:22_

---

_Referenced in [astral-sh/ruff#11774](../../astral-sh/ruff/pulls/11774.md) on 2024-06-06 09:58_

---

_Closed by @charliermarsh on 2024-06-07 03:10_

---
