```yaml
number: 13234
title: Fix incorrect venv invalidation for pre-release Python versions
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-version-comp
created_at: 2025-04-30T15:34:42Z
updated_at: 2025-05-04T12:26:21Z
url: https://github.com/astral-sh/uv/pull/13234
synced_at: 2026-01-12T16:10:36Z
```

# Fix incorrect venv invalidation for pre-release Python versions

---

_@zanieb_

I think this regressed in https://github.com/astral-sh/uv/pull/13027 — I misunderstood what versions could be represented in the `pyvenv.cfg` (I assumed they _never_ included pre-release components).

Closes #13233 

---

_Label `bug` added by @zanieb on 2025-04-30 15:34_

---

_Comment by @zanieb on 2025-04-30 15:37_

This isn't quite right yet.

---

_Comment by @zanieb on 2025-04-30 15:42_

I tested this with the reproduction from the issue. I may add a test case separately if we want to release sooner.

---

_Comment by @zanieb on 2025-04-30 15:51_

In hindsight, `uv run --no-sync` should probably never delete the environment like this? See https://github.com/astral-sh/uv/issues/13235

---

_Merged by @zanieb on 2025-04-30 15:55_

---

_Closed by @zanieb on 2025-04-30 15:55_

---

_Branch deleted on 2025-04-30 15:55_

---
