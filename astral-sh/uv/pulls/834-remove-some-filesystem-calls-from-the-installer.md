```yaml
number: 834
title: Remove some filesystem calls from the installer
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/entry
created_at: 2024-01-08T17:22:23Z
updated_at: 2024-01-08T17:59:02Z
url: https://github.com/astral-sh/uv/pull/834
synced_at: 2026-01-12T16:04:13Z
```

# Remove some filesystem calls from the installer

---

_@charliermarsh_

Noticed these when working on something unrelated. Generally:

- Prefer `entry.file_type()` over `entry.path().is_file()` or similar, as the former is almost always free on Unix.
- Call `entry.path()` once, since it allocates internally (returns a `PathBuf`).

---

_Label `internal` added by @charliermarsh on 2024-01-08 17:22_

---

_Merged by @charliermarsh on 2024-01-08 17:59_

---

_Closed by @charliermarsh on 2024-01-08 17:59_

---

_Branch deleted on 2024-01-08 17:59_

---
