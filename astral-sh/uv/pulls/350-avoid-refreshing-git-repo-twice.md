```yaml
number: 350
title: Avoid refreshing Git repo twice
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/hash
created_at: 2023-11-07T02:49:05Z
updated_at: 2023-11-07T02:52:16Z
url: https://github.com/astral-sh/uv/pull/350
synced_at: 2026-01-12T16:03:53Z
```

# Avoid refreshing Git repo twice

---

_@charliermarsh_

This was a bug in the Git code (that I wrote, not from Cargo) -- when we `precise` the reference, we should store the resolved commit.

---

_Merged by @charliermarsh on 2023-11-07 02:52_

---

_Closed by @charliermarsh on 2023-11-07 02:52_

---

_Branch deleted on 2023-11-07 02:52_

---
