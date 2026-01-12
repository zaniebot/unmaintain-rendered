```yaml
number: 16394
title: Do not error when a virtual environment directory cannot be removed due to a busy error
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/remove-dir
created_at: 2025-10-21T17:38:59Z
updated_at: 2025-10-23T20:08:55Z
url: https://github.com/astral-sh/uv/pull/16394
synced_at: 2026-01-12T16:12:14Z
```

# Do not error when a virtual environment directory cannot be removed due to a busy error

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/16218

This occurs when using a mounted file system in Docker.

We're almost always removing a virtual environment to replace it, and removing the parent directory isn't necessary for that.

---

_Label `enhancement` added by @zanieb on 2025-10-21 17:39_

---

_Marked ready for review by @zanieb on 2025-10-21 18:08_

---

_Merged by @zanieb on 2025-10-23 20:08_

---

_Closed by @zanieb on 2025-10-23 20:08_

---

_Branch deleted on 2025-10-23 20:08_

---
