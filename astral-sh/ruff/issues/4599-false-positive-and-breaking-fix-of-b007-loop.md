```yaml
number: 4599
title: "False-positive and breaking fix of B007: Loop control variable not used within loop body"
type: issue
state: closed
author: bersbersbers
labels: []
assignees: []
created_at: 2023-05-23T13:39:57Z
updated_at: 2023-05-23T15:44:01Z
url: https://github.com/astral-sh/ruff/issues/4599
synced_at: 2026-01-10T11:09:47Z
```

# False-positive and breaking fix of B007: Loop control variable not used within loop body

---

_Issue opened by @bersbersbers on 2023-05-23 13:39_

`bug.py`
```python
# python bug.py  # works
# ruff bug.py --fix --select B007
# python bug.py  # fails

for element in [0]:
    break
else:
    element = -1
print(element)
```

ruff 0.0.269 with B007 breaks this file by renaming `element` to `_element`, but it is in fact used after the loop.


---

_Comment by @charliermarsh on 2023-05-23 14:39_

We explicitly guard against this, so must be a bug somewhere in that implementation.

---

_Closed by @charliermarsh on 2023-05-23 15:44_

---
