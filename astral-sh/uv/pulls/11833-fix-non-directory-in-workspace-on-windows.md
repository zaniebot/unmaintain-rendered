```yaml
number: 11833
title: Fix non-directory in workspace on Windows
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/fix-gh11793
created_at: 2025-02-27T15:07:08Z
updated_at: 2025-02-28T12:40:21Z
url: https://github.com/astral-sh/uv/pull/11833
synced_at: 2026-01-12T16:10:01Z
```

# Fix non-directory in workspace on Windows

---

_@konstin_

Fixes #11793

On Windows, trying to read a file inside what is not a directory but another file results in a not found error, while on Unix we get a not a directory error. We check explicitly if something included in a workspace glob is a non-directory to fix the behavior on Windows.

---

_Label `bug` added by @konstin on 2025-02-27 15:07_

---

_@charliermarsh reviewed on 2025-02-27 16:17_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/workspace.rs`:736 on 2025-02-27 16:17_

Could we do this inside of the `if err.kind() == std::io::ErrorKind::NotFound` branch instead, to avoid paying this cost in the common case?

---

_Merged by @konstin on 2025-02-28 12:40_

---

_Closed by @konstin on 2025-02-28 12:40_

---

_Branch deleted on 2025-02-28 12:40_

---
