```yaml
number: 1834
title: "Support for-else loops in `SIM110` and `SIM111`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/else
created_at: 2023-01-12T22:04:23Z
updated_at: 2023-01-12T22:05:00Z
url: https://github.com/astral-sh/ruff/pull/1834
synced_at: 2026-01-12T05:36:32Z
```

# Support for-else loops in `SIM110` and `SIM111`

---

_Pull request opened by @charliermarsh on 2023-01-12 22:04_

This PR adds support for `SIM110` and `SIM111` simplifications of the form:

```py
def f():
    # SIM110
    for x in iterable:
        if check(x):
            return True
    else:
        return False
```

---

_Merged by @charliermarsh on 2023-01-12 22:04_

---

_Closed by @charliermarsh on 2023-01-12 22:04_

---

_Branch deleted on 2023-01-12 22:05_

---
