```yaml
number: 16781
title: Document the new behavior for free-threaded python versions
type: pull_request
state: merged
author: pythonweb2
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-11-19T18:32:30Z
updated_at: 2025-11-20T15:28:07Z
url: https://github.com/astral-sh/uv/pull/16781
synced_at: 2026-01-12T16:12:26Z
```

# Document the new behavior for free-threaded python versions

---

_@pythonweb2_

## Summary

I noticed that after first installing the free-threaded version, then the gil version of 3.14, I wasn't able
to install greenlet, because it doesn't ship with wheels for the free-threaded version (I think it isn't
safe for it to use that interpreter). I noticed that the change made in 3.14 wasn't updated in the docs.

## Test Plan

N/A


---

_Label `documentation` added by @zanieb on 2025-11-20 14:22_

---

_Comment by @zanieb on 2025-11-20 14:22_

Thanks!

---

_Merged by @zanieb on 2025-11-20 14:58_

---

_Closed by @zanieb on 2025-11-20 14:58_

---

_Branch deleted on 2025-11-20 15:28_

---
