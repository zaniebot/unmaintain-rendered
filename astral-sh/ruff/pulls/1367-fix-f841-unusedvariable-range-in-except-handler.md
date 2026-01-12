```yaml
number: 1367
title: "Fix F841 (`UnusedVariable`) range in except handler"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: fix-F841-location-in-except-handler
created_at: 2022-12-25T03:20:04Z
updated_at: 2022-12-25T05:05:39Z
url: https://github.com/astral-sh/ruff/pull/1367
synced_at: 2026-01-12T15:55:06Z
```

# Fix F841 (`UnusedVariable`) range in except handler

---

_@harupy_

Currently, `F841` spans the entire except handler, which is undesired. This PR fixes it.

## Example

```python
try:
    pass
except ValueError as e:
    pass

```

### Before

```
a.py:3:1: F841 Local variable `e` is assigned to but never used
  |
3 | / except ValueError as e:
4 | |     pass
  | |________^ F841
  |
```

### After

```
a.py:3:22: F841 Local variable `e` is assigned to but never used
  |
3 | except ValueError as e:
  |                      ^ F841
  |
```

---

_Merged by @charliermarsh on 2022-12-25 03:55_

---

_Closed by @charliermarsh on 2022-12-25 03:55_

---

_Comment by @charliermarsh on 2022-12-25 03:56_

Awesome, these range fixes are really impactful.

---

_Branch deleted on 2022-12-25 05:05_

---
