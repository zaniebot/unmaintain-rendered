```yaml
number: 14337
title: "Use the canonical `ImplementationName` -> `&str` implementation"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/impl-into
created_at: 2025-06-27T23:30:58Z
updated_at: 2025-06-28T14:42:19Z
url: https://github.com/astral-sh/uv/pull/14337
synced_at: 2026-01-10T06:53:01Z
```

# Use the canonical `ImplementationName` -> `&str` implementation

---

_Pull request opened by @zanieb on 2025-06-27 23:30_

Motivated by some code duplication highlighted in https://github.com/astral-sh/uv/pull/14201, I noticed we weren't taking advantage of the existing implementation for casting to a str here. Unfortunately, we do need a special case for CPython still.

---

_Label `internal` added by @zanieb on 2025-06-27 23:30_

---

_Marked ready for review by @zanieb on 2025-06-28 03:10_

---

_@charliermarsh approved on 2025-06-28 12:51_

---

_Merged by @zanieb on 2025-06-28 14:42_

---

_Closed by @zanieb on 2025-06-28 14:42_

---

_Branch deleted on 2025-06-28 14:42_

---
