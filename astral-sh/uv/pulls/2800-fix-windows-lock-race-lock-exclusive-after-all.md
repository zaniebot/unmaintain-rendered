```yaml
number: 2800
title: "Fix windows lock race: Lock exclusive after all try lock errors"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/add-filename-to-lock-error
created_at: 2024-04-03T08:56:52Z
updated_at: 2024-04-03T16:00:17Z
url: https://github.com/astral-sh/uv/pull/2800
synced_at: 2026-01-12T16:05:13Z
```

# Fix windows lock race: Lock exclusive after all try lock errors

---

_@konstin_

We don't know what kind of error the OS gives us on `try_lock_exclusive` with an already locked file, so we assume all those errors are an already locked file and call `lock_exclusive`.

For example the windows error:

```
Os {
    code: 33,
    kind: Uncategorized,
    message: "The process cannot access the file because another process has locked a portion of the file.",
}
```

Fixes #2767

---

_Label `bug` added by @konstin on 2024-04-03 08:56_

---

_Merged by @konstin on 2024-04-03 14:02_

---

_Closed by @konstin on 2024-04-03 14:02_

---

_Branch deleted on 2024-04-03 14:02_

---

_@zanieb reviewed on 2024-04-03 14:50_

---

_Review comment by @zanieb on `crates/uv-fs/src/lib.rs`:297 on 2024-04-03 14:50_

@konstin can you open an upstream issue to add?

---

_@konstin reviewed on 2024-04-03 16:00_

---

_Review comment by @konstin on `crates/uv-fs/src/lib.rs`:297 on 2024-04-03 16:00_

I don't think that's in scope for either project 

---
