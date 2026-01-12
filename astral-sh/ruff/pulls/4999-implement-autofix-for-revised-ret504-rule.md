```yaml
number: 4999
title: "Implement autofix for revised `RET504` rule"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
  - fixes
assignees: []
merged: true
base: main
head: charlie/return-fix
created_at: 2023-06-10T03:24:06Z
updated_at: 2023-06-10T04:32:05Z
url: https://github.com/astral-sh/ruff/pull/4999
synced_at: 2026-01-12T03:43:29Z
```

# Implement autofix for revised `RET504` rule

---

_Pull request opened by @charliermarsh on 2023-06-10 03:24_

## Summary

This PR enables autofix for the revised `RET504` rule, by changing:

```py
def f():
    x = 1
    return x
```

...to:

```py
def f():
    return 1
```

Closes #2263.

Closes #2788.


---

_Label `rule` added by @charliermarsh on 2023-06-10 03:24_

---

_Label `autofix` added by @charliermarsh on 2023-06-10 03:24_

---

_Merged by @charliermarsh on 2023-06-10 04:32_

---

_Closed by @charliermarsh on 2023-06-10 04:32_

---

_Branch deleted on 2023-06-10 04:32_

---
