```yaml
number: 7259
title: E203 fix should leave equal amounts of space on either side of a slice operator
type: issue
state: closed
author: bersbersbers
labels:
  - rule
  - accepted
assignees: []
created_at: 2023-09-10T15:46:42Z
updated_at: 2023-11-13T17:16:14Z
url: https://github.com/astral-sh/ruff/issues/7259
synced_at: 2026-01-12T15:54:46Z
```

# E203 fix should leave equal amounts of space on either side of a slice operator

---

_@bersbersbers_

```python
ham[lower + offset : upper + offset]
```

`ruff --fix --select E203 bug.py` (v0.0.287) gives

```python
ham[lower + offset: upper + offset]
```

which is discouraged by PEP 8:

> However, in a slice the colon acts like a binary operator, and should have equal amounts on either side (treating it as the operator with the lowest priority).

https://peps.python.org/pep-0008/#pet-peeves

So, `ham[lower + offset: upper + offset]` is incorrect. I would argue that `ham[lower + offset : upper + offset]` (which is what `black` outputs) should not be flagged by `E203`; and if so, *both* spaces should be removed (but I still argue to remove none at all).

---

_Comment by @charliermarsh on 2023-09-11 23:13_

Our behavior is consistent with pycodestyle, but I am inclined to support this (though it's low priority).

---

_Label `rule` added by @charliermarsh on 2023-09-11 23:13_

---

_Label `accepted` added by @charliermarsh on 2023-09-11 23:13_

---

_Comment by @oefe on 2023-09-17 14:15_

It's also inconsistent with black, which puts spaces on both sides.
(Though I would argue that if you're using black, you don't that rule anyway.)

---

_Comment by @gertvdijk on 2023-09-21 19:49_

I think this is a duplicate of https://github.com/astral-sh/ruff/issues/6861, but I'm happy to see it's considered/accepted now. 😃 

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-13 05:04_

---

_Comment by @charliermarsh on 2023-11-13 14:51_

Gonna look at fixing this today.

---

_Closed by @charliermarsh on 2023-11-13 17:16_

---
