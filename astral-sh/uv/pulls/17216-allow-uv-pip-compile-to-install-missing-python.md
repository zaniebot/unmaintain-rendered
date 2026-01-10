```yaml
number: 17216
title: "Allow `uv pip compile` to install missing python interpreters in cases where it would otherwise fail"
type: pull_request
state: merged
author: EliteTK
labels:
  - enhancement
assignees: []
merged: true
base: main
head: tk/pip-compile-missing-py
created_at: 2025-12-22T15:39:20Z
updated_at: 2025-12-22T16:29:51Z
url: https://github.com/astral-sh/uv/pull/17216
synced_at: 2026-01-10T05:49:14Z
```

# Allow `uv pip compile` to install missing python interpreters in cases where it would otherwise fail

---

_Pull request opened by @EliteTK on 2025-12-22 15:39_

## Summary

Partially address #16709.

Previously, if cornered, `pip compile` would fail when the requested python interpreter couldn't be found (more details in the issue and comments), and now in those cases it will download it.

## Test Plan

Added an integration test for this case.

---

_Review requested from @zanieb by @EliteTK on 2025-12-22 15:39_

---

_Label `enhancement` added by @EliteTK on 2025-12-22 15:39_

---

_@zanieb approved on 2025-12-22 15:45_

---

_Renamed from "`pip compile` install a python interpreter when necessary" to "Allow `uv pip compile` to install missing python interpreters in cases where it would otherwise fail" by @EliteTK on 2025-12-22 15:53_

---

_Merged by @EliteTK on 2025-12-22 16:29_

---

_Closed by @EliteTK on 2025-12-22 16:29_

---

_Branch deleted on 2025-12-22 16:29_

---
