```yaml
number: 668
title: Modify PEP 508 cursor to use byte offsets
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/parser
created_at: 2023-12-15T22:01:33Z
updated_at: 2023-12-15T22:05:29Z
url: https://github.com/astral-sh/uv/pull/668
synced_at: 2026-01-12T16:04:06Z
```

# Modify PEP 508 cursor to use byte offsets

---

_@charliermarsh_

This enables us to remove a number of allocations (in particular, `peek_while` and `take_while` no longer allocate). It also makes it trivial to move the cursor to a new location, since you can just slice and call `.chars()`. At present, moving to a new location would require converting the iterator to a string, then back to a character iterator.


---

_Merged by @charliermarsh on 2023-12-15 22:05_

---

_Closed by @charliermarsh on 2023-12-15 22:05_

---

_Branch deleted on 2023-12-15 22:05_

---
