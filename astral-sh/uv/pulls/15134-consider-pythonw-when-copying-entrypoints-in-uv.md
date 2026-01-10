```yaml
number: 15134
title: Consider pythonw when copying entrypoints in uv run
type: pull_request
state: merged
author: samypr100
labels:
  - bug
assignees: []
merged: true
base: main
head: gui-overlays
created_at: 2025-08-07T13:30:02Z
updated_at: 2025-08-07T15:47:54Z
url: https://github.com/astral-sh/uv/pull/15134
synced_at: 2026-01-10T06:44:33Z
```

# Consider pythonw when copying entrypoints in uv run

---

_Pull request opened by @samypr100 on 2025-08-07 13:30_


## Summary

Follow up from https://github.com/astral-sh/uv/pull/15068#discussion_r2258586926

It seems when copying entrypoints we're ignoring whether it was pythonw vs not.

## Test Plan

Updated existing test.


---

_@zanieb approved on 2025-08-07 13:31_

Eek, thank you!

---

_Label `bug` added by @zanieb on 2025-08-07 13:32_

---

_Marked ready for review by @samypr100 on 2025-08-07 14:05_

---

_Merged by @zanieb on 2025-08-07 15:06_

---

_Closed by @zanieb on 2025-08-07 15:06_

---

_Branch deleted on 2025-08-07 15:47_

---
