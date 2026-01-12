```yaml
number: 16845
title: Use 0o666 permissions for flock files instead of 0o777
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/lock-perms
created_at: 2025-11-25T15:08:29Z
updated_at: 2025-12-01T18:09:45Z
url: https://github.com/astral-sh/uv/pull/16845
synced_at: 2026-01-12T16:12:28Z
```

# Use 0o666 permissions for flock files instead of 0o777

---

_@zanieb_

This removes executable permissions while retaining global read / global write.

It's been suggested we should use 0o644 instead, dropping the global write permissions (i.e., just the owner can write), but since we're taking an exclusive lock I don't think that would work and we'd regress the issue that was solved by updating the permissions. I think we'll need to revisit the locking scheme if that's the goal, but regardless, this seems like a net improvement.

---

_Marked ready for review by @zanieb on 2025-11-26 15:51_

---

_Comment by @konstin on 2025-11-27 14:54_

@geofft Do you know what permissions we need so that different processes, e.g. running in different docker containers, can use the same lockfile?

---

_@konstin approved on 2025-11-27 14:55_

666 is clearly better than 777, I don't know POSIX well enough to know if we can go tighter than that

---

_Label `enhancement` added by @konstin on 2025-11-27 14:55_

---

_Merged by @zanieb on 2025-12-01 18:09_

---

_Closed by @zanieb on 2025-12-01 18:09_

---

_Branch deleted on 2025-12-01 18:09_

---
