```yaml
number: 2885
title: B904 not triggered in else block 
type: issue
state: closed
author: tekumara
labels: []
assignees: []
created_at: 2023-02-14T03:45:57Z
updated_at: 2023-02-14T03:58:17Z
url: https://github.com/astral-sh/ruff/issues/2885
synced_at: 2026-01-12T15:54:43Z
```

# B904 not triggered in else block 

---

_@tekumara_

```python
try:
    ...
except Exception as e:
    if ...:
        raise RuntimeError("boom!")
    else:
        raise RuntimeError("bang!")
```

ruff only identifies B904 on line 5, and not on line 7:
```
:5:9: B904 Within an except clause, raise exceptions with `raise ... from err` or `raise ... from None` to distinguish them from errors in exception handling
```

ruff 0.0.246

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-14 03:52_

---

_Closed by @charliermarsh on 2023-02-14 03:58_

---
