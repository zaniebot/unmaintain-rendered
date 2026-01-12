```yaml
number: 2627
title: Avoid non-recursion in nested typing function calls
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/arg001
created_at: 2023-02-07T16:20:15Z
updated_at: 2023-02-07T16:22:18Z
url: https://github.com/astral-sh/ruff/pull/2627
synced_at: 2026-01-12T15:55:08Z
```

# Avoid non-recursion in nested typing function calls

---

_@charliermarsh_

Previously, we treated the outer `Call` in `cast(...)(x=x)` as `cast`, whereas in reality only the _inner_ call is `cast`.

Closes #2620.

---

_Merged by @charliermarsh on 2023-02-07 16:21_

---

_Closed by @charliermarsh on 2023-02-07 16:21_

---

_Branch deleted on 2023-02-07 16:21_

---

_Label `bug` added by @charliermarsh on 2023-02-07 16:22_

---
