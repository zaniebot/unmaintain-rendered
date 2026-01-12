```yaml
number: 1294
title: Avoid RET504 errors for intermediary function calls
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/RET504
created_at: 2022-12-20T00:47:44Z
updated_at: 2022-12-20T00:48:10Z
url: https://github.com/astral-sh/ruff/pull/1294
synced_at: 2026-01-12T05:36:31Z
```

# Avoid RET504 errors for intermediary function calls

---

_Pull request opened by @charliermarsh on 2022-12-20 00:47_

This has led to enough false-positives that I want to at least avoid flagging it when we have function calls between the assignment and the return (which handles the filed cases).

Resolves #1290, #1233.

---

_Merged by @charliermarsh on 2022-12-20 00:48_

---

_Closed by @charliermarsh on 2022-12-20 00:48_

---

_Branch deleted on 2022-12-20 00:48_

---
