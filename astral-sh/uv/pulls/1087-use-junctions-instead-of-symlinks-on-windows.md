```yaml
number: 1087
title: Use junctions instead of symlinks on Windows
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/junction
created_at: 2024-01-25T01:26:50Z
updated_at: 2024-01-25T13:43:33Z
url: https://github.com/astral-sh/uv/pull/1087
synced_at: 2026-01-12T16:04:25Z
```

# Use junctions instead of symlinks on Windows

---

_@charliermarsh_

## Summary

When we unzip wheels in the cache, we write the directories out to an `archive-v0` bucket, and then symlink into that bucket from the `wheels-v0` and `built-wheels-v0` buckets.

On Windows, symlinks are not well supported. Specifically, they need to be explicitly enabled by the user. So, instead of symlinks, we now use junctions, which are well-supported on Windows, and allow you to (effectively) symlink a directory to another directory. This PR implements said junction support, which gets the core installer working on Windows.

In the past, we also used symlinks to implement another primitive: we wanted to be able to replace a directory "atomically" (I put "atomically" in quotes because I don't know if it's actually a guaranteed atomic operation), in case someone was trying to use the directory while we were replacing it (as opposed to deleting the directory, then moving it into place).

On Windows, it doesn't appear to be possible to atomically replace a junction. So instead, I'm using a new design, whereby the cache always returns canonicalized paths. We know these canonicalized paths are unique and won't be replaced, so they're safe for writers to rely on. In general, when we write new data to the cache, we now return the canonicalized path. When we read from the cache, and try to identify (e.g.) the set of wheels available to us, we canonicalize the links immediately and consider them non-existent if that operation fails.

Closes #1085.

---

_Review requested from @konstin by @charliermarsh on 2024-01-25 01:35_

---

_Label `bug` added by @charliermarsh on 2024-01-25 01:35_

---

_Label `windows` added by @charliermarsh on 2024-01-25 01:35_

---

_Marked ready for review by @charliermarsh on 2024-01-25 01:46_

---

_Review comment by @konstin on `crates/puffin-fs/src/lib.rs`:18 on 2024-01-25 08:55_

Why do have to delete again here?

---

_@konstin approved on 2024-01-25 08:55_

---

_Comment by @konstin on 2024-01-25 09:06_

Merging since this unblocks me

---

_Merged by @konstin on 2024-01-25 09:06_

---

_Closed by @konstin on 2024-01-25 09:06_

---

_Branch deleted on 2024-01-25 09:06_

---

_@charliermarsh reviewed on 2024-01-25 13:43_

---

_Review comment by @charliermarsh on `crates/puffin-fs/src/lib.rs`:18 on 2024-01-25 13:43_

Deleting the reparse point does not delete the file or directly (this is in the docs).

In my testing, deleting the directory without deleting the reparse point gave me an error. And so did deleting the reparse point without deleting the directory.

---
