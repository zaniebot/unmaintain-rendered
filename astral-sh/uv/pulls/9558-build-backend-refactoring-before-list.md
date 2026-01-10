```yaml
number: 9558
title: "Build backend: Refactoring before list"
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/build-backend-list
created_at: 2024-12-01T22:42:29Z
updated_at: 2024-12-02T15:57:05Z
url: https://github.com/astral-sh/uv/pull/9558
synced_at: 2026-01-10T12:00:00Z
```

# Build backend: Refactoring before list

---

_Pull request opened by @konstin on 2024-12-01 22:42_

For listing files, we first use a directory writer for source dists, which we will use for collecting the filenames instead of writing the archive in the future. I've split breaking `lib.rs` of uv-build-backend into modules into the next PR.

No logic changes, only restructuring.

Best reviewed commit-by-commit

---

_Label `preview` added by @konstin on 2024-12-01 22:42_

---

_Review comment by @konstin on `crates/uv-build-backend/src/fs_write_dispatcher.rs`:25 on 2024-12-01 22:43_

This is an awfully uninformative name. It's like a virtual fs, but append-only and it's actually more our sink than a real fs.

---

_@konstin reviewed on 2024-12-01 22:43_

---

_Marked ready for review by @konstin on 2024-12-02 15:36_

---

_@konstin reviewed on 2024-12-02 15:46_

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:306 on 2024-12-02 15:46_

Learning: If you set directories to 644, you get a lot of permission errors, the executable bit is the list bit, which is distinct from the read permission.

---

_Merged by @konstin on 2024-12-02 15:57_

---

_Closed by @konstin on 2024-12-02 15:57_

---

_Branch deleted on 2024-12-02 15:57_

---
