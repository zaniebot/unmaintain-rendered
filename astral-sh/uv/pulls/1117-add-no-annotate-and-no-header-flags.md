```yaml
number: 1117
title: "Add `--no-annotate` and `--no-header` flags"
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/annotate
created_at: 2024-01-26T02:35:18Z
updated_at: 2024-01-26T12:14:19Z
url: https://github.com/astral-sh/uv/pull/1117
synced_at: 2026-01-12T16:04:26Z
```

# Add `--no-annotate` and `--no-header` flags

---

_@charliermarsh_

Closes #1107.
Closes #1108.

---

_Review requested from @zanieb by @charliermarsh on 2024-01-26 02:35_

---

_Label `compatibility` added by @charliermarsh on 2024-01-26 02:35_

---

_@charliermarsh reviewed on 2024-01-26 02:35_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolution.rs`:293 on 2024-01-26 02:35_

This is all just indented.

---

_@charliermarsh reviewed on 2024-01-26 02:36_

---

_Review comment by @charliermarsh on `crates/puffin/src/main.rs`:209 on 2024-01-26 02:36_

So we now support `--no-header` but not `--header` (the default), which seems ok.

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolution.rs`:222 on 2024-01-26 08:35_

Source is ambiguous, could we say something like "which dependency requested the package"?

---

_@konstin approved on 2024-01-26 08:36_

---

_Merged by @charliermarsh on 2024-01-26 12:14_

---

_Closed by @charliermarsh on 2024-01-26 12:14_

---

_Branch deleted on 2024-01-26 12:14_

---
