```yaml
number: 3286
title: Avoid PEP 585 rewrites when builtins are shadowed
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/builtin
created_at: 2023-02-28T23:22:18Z
updated_at: 2023-02-28T23:25:43Z
url: https://github.com/astral-sh/ruff/pull/3286
synced_at: 2026-01-12T04:39:44Z
```

# Avoid PEP 585 rewrites when builtins are shadowed

---

_Pull request opened by @charliermarsh on 2023-02-28 23:22_

This just avoids attempting to fix these -- so it doesn't solve the issue as-described, but it avoids breaking the user's code.

Closes #3284.


---

_Label `bug` added by @charliermarsh on 2023-02-28 23:22_

---

_Merged by @charliermarsh on 2023-02-28 23:25_

---

_Closed by @charliermarsh on 2023-02-28 23:25_

---

_Branch deleted on 2023-02-28 23:25_

---
