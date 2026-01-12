```yaml
number: 10672
title: Log source file on compile timeout
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/log-source-file-in-timeout
created_at: 2025-01-16T10:43:32Z
updated_at: 2025-01-16T15:01:25Z
url: https://github.com/astral-sh/uv/pull/10672
synced_at: 2026-01-12T16:09:25Z
```

# Log source file on compile timeout

---

_@konstin_

Log the file that failed to bytecode compile when encountering a timeout for debugging #6105 better.

[sysinfo](https://lib.rs/crates/sysinfo) would give us the option to report memory usage too, but i'm hesitant to add a dependency just for the error path.

---

_Label `error messages` added by @konstin on 2025-01-16 10:43_

---

_@Gankra approved on 2025-01-16 14:58_

nice

---

_Merged by @Gankra on 2025-01-16 15:01_

---

_Closed by @Gankra on 2025-01-16 15:01_

---

_Branch deleted on 2025-01-16 15:01_

---
