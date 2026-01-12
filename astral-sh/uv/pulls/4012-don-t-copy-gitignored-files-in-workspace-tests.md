```yaml
number: 4012
title: "Don't copy gitignored files in workspace tests"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/copy-dir-ignore
created_at: 2024-06-04T12:45:30Z
updated_at: 2024-06-04T12:58:08Z
url: https://github.com/astral-sh/uv/pull/4012
synced_at: 2026-01-12T16:05:59Z
```

# Don't copy gitignored files in workspace tests

---

_@konstin_

The workspace test directories can be used both in tests and directly for developing/debugging. In the latter, we shouldn't copy the venv and the lockfile when running tests. Using the ignore crate over manual recursion we exclude those files.

---

_Label `internal` added by @konstin on 2024-06-04 12:45_

---

_Merged by @konstin on 2024-06-04 12:58_

---

_Closed by @konstin on 2024-06-04 12:58_

---

_Branch deleted on 2024-06-04 12:58_

---
