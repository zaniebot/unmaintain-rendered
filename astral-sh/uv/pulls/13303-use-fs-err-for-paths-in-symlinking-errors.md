```yaml
number: 13303
title: "Use `fs_err` for paths in symlinking errors"
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/use-fs-err-for-symlinks
created_at: 2025-05-05T16:19:37Z
updated_at: 2025-05-05T16:29:28Z
url: https://github.com/astral-sh/uv/pull/13303
synced_at: 2026-01-12T16:10:38Z
```

# Use `fs_err` for paths in symlinking errors

---

_@konstin_

In #13302, there was an IO error without context. This error seems to be caused by a symlink error. Switching as symlinking to `fs_err` ensures these errors will carry context in the future.

---

_Label `enhancement` added by @konstin on 2025-05-05 16:19_

---

_@zanieb approved on 2025-05-05 16:25_

---

_Label `error messages` added by @zanieb on 2025-05-05 16:25_

---

_Label `enhancement` removed by @konstin on 2025-05-05 16:27_

---

_Merged by @konstin on 2025-05-05 16:29_

---

_Closed by @konstin on 2025-05-05 16:29_

---

_Branch deleted on 2025-05-05 16:29_

---
