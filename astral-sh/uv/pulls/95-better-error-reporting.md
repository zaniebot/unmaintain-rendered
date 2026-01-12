```yaml
number: 95
title: Better error reporting
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: error_messages
created_at: 2023-10-12T18:41:45Z
updated_at: 2023-10-16T02:15:12Z
url: https://github.com/astral-sh/uv/pull/95
synced_at: 2026-01-12T16:03:45Z
```

# Better error reporting

---

_@konstin_

The main change is to print the whole error chain. We can combine this with adding `.context` to distinct phases to be able to locate crashes without having to use a debugger.

---

_Review requested from @charliermarsh by @konstin on 2023-10-12 18:44_

---

_Merged by @charliermarsh on 2023-10-16 02:15_

---

_Closed by @charliermarsh on 2023-10-16 02:15_

---

_Branch deleted on 2023-10-16 02:15_

---
