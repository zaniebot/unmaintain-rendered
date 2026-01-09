---
number: 5780
title: Comment placement of expressions with keywords
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
assignees: []
created_at: 2023-07-15T15:00:03Z
updated_at: 2023-09-08T07:25:39Z
url: https://github.com/astral-sh/ruff/issues/5780
synced_at: 2026-01-07T13:12:15-06:00
---

# Comment placement of expressions with keywords

---

_Issue opened by @MichaReiser on 2023-07-15 15:00_

We have a few nodes that are separated by a keyword. For example:

```python
with (x as y) # x and y are separated by the as keyword
except x as e: # Same
call(a, b) # a and b are separated by a comma
...
```

It would be nice if we could unify the comment placement logic for all of these. 

> i wonder if an abstraction with two nodes with a single token in between makes sense, which would also force us to handle them consistently. We have the same pattern in slices (x[a:b]), argument lists x(a,b) and binary expressions (a+b). (also this feature is inconsistent because with (a as b) works but except (a as b) doesn't). 
[Original Message](https://github.com/astral-sh/ruff/pull/5758/files/5e98a87684a79cd297756072870d5b7142498ffc#r1263948323) from @konstin 


This task aims to explore if generalizing the logic is possible and, if so, implement the generalization. 

---

_Referenced in [astral-sh/ruff#4798](../../astral-sh/ruff/issues/4798.md) on 2023-07-15 15:00_

---

_Referenced in [astral-sh/ruff#5758](../../astral-sh/ruff/pulls/5758.md) on 2023-07-15 15:02_

---

_Label `formatter` added by @konstin on 2023-07-15 19:26_

---

_Comment by @MichaReiser on 2023-09-08 07:25_

Closing as unplanned. 

---

_Closed by @MichaReiser on 2023-09-08 07:25_

---
