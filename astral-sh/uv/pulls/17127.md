```yaml
number: 17127
title: "Relax error when using uv add with `UV_GIT_LFS` set"
type: pull_request
state: merged
author: samypr100
labels:
  - bug
assignees: []
merged: true
base: main
head: track-lfs-arg-vs-env
created_at: 2025-12-13T19:01:05Z
updated_at: 2025-12-15T20:58:49Z
url: https://github.com/astral-sh/uv/pull/17127
synced_at: 2026-01-10T05:49:14Z
```

# Relax error when using uv add with `UV_GIT_LFS` set

---

_Pull request opened by @samypr100 on 2025-12-13 19:01_

## Summary

Closes https://github.com/astral-sh/uv/issues/17083

Previously having `UV_GIT_LFS` set would cause an error when adding a non-git requirement such as ```error: `requirement` did not resolve to a Git repository, but a Git extension (`--lfs`) was provided.```

## Test Plan

Additional test has been added.


---

_Marked ready for review by @samypr100 on 2025-12-13 19:46_

---

_Converted to draft by @samypr100 on 2025-12-13 20:17_

---

_Marked ready for review by @samypr100 on 2025-12-13 22:32_

---

_Renamed from "Differentiate between `--lfs` and `UV_GIT_LFS`" to "Relax error when using uv add with `UV_GIT_LFS` set" by @samypr100 on 2025-12-14 22:53_

---

_@zanieb approved on 2025-12-15 14:26_

---

_Merged by @zanieb on 2025-12-15 14:26_

---

_Closed by @zanieb on 2025-12-15 14:26_

---

_Label `bug` added by @zanieb on 2025-12-15 14:26_

---

_Branch deleted on 2025-12-15 20:58_

---
