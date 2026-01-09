---
number: 4356
title: "E712 : Wrong fix suggested in presence of `numpy.bool_`"
type: issue
state: closed
author: kesinger-idexx
labels: []
assignees: []
created_at: 2023-05-10T18:36:20Z
updated_at: 2023-05-10T20:12:54Z
url: https://github.com/astral-sh/ruff/issues/4356
synced_at: 2026-01-07T13:12:14-06:00
---

# E712 : Wrong fix suggested in presence of `numpy.bool_`

---

_Issue opened by @kesinger-idexx on 2023-05-10 18:36_

Using ruff 0.0.265.


In this code, the two if-blocks are NOT equivalent:

    def f(x):
        # x is a np.ndarray with at least 2 dimensions
        return x[0, 0] > 0

    x = np.eye(2)
    a = f(x)
    if a == True:
        print("a==True")
    if a is True:
        print("a is True")
        assert(False)


What's going on is `type(a)==numpy.bool_` which can be compared to True/False via `==` but it is not referentially the same.


---

_Comment by @zanieb on 2023-05-10 18:40_

Duplicate of https://github.com/charliermarsh/ruff/issues/2443

I'll make a tracking issue for all `is`/`==` problems

---

_Closed by @charliermarsh on 2023-05-10 20:12_

---

_Referenced in [astral-sh/ruff#4560](../../astral-sh/ruff/issues/4560.md) on 2023-05-21 15:51_

---
