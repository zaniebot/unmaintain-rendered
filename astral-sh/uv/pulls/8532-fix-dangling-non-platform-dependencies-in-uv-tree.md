```yaml
number: 8532
title: "Fix dangling non-platform dependencies in `uv tree`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/uni
created_at: 2024-10-24T16:22:56Z
updated_at: 2024-10-24T16:39:02Z
url: https://github.com/astral-sh/uv/pull/8532
synced_at: 2026-01-12T16:08:21Z
```

# Fix dangling non-platform dependencies in `uv tree`

---

_@charliermarsh_

## Summary

We were including dependencies that were only included by a dependency that isn't relevant on the current platform (i.e., we were enforcing the "current environment" at one level, but not transitively).

Closes https://github.com/astral-sh/uv/issues/8516.


---

_Label `bug` added by @charliermarsh on 2024-10-24 16:23_

---

_Merged by @charliermarsh on 2024-10-24 16:39_

---

_Closed by @charliermarsh on 2024-10-24 16:39_

---

_Branch deleted on 2024-10-24 16:39_

---
