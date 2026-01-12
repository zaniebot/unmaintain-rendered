```yaml
number: 14731
title: "Match `--bounds` formatting for `uv_build` bounds in `uv init`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: codex/fix-issue-#14724
created_at: 2025-07-18T16:03:31Z
updated_at: 2025-07-21T09:48:39Z
url: https://github.com/astral-sh/uv/pull/14731
synced_at: 2026-01-12T16:11:22Z
```

# Match `--bounds` formatting for `uv_build` bounds in `uv init`

---

_@zanieb_

Closes #14724 

https://chatgpt.com/codex/tasks/task_e_687a53ba646c8331baa4140c5b2bec70

---

_Label `codex` added by @zanieb on 2025-07-18 16:03_

---

_Renamed from "Fix uv_build upper bound formatting" to "Always include the patch version in `uv_build` upper bounds" by @zanieb on 2025-07-18 16:22_

---

_Marked ready for review by @zanieb on 2025-07-18 16:22_

---

_Review requested from @konstin by @zanieb on 2025-07-18 16:26_

---

_Comment by @zanieb on 2025-07-20 17:17_

I don't really prefer this, but defer to you @konstin 

---

_Comment by @konstin on 2025-07-21 09:40_

The codex log seems private.

I prefer matching `--bounds` here for consistency; I've changed the logic so that it doesn't just add a zero, but matches the length as `--bounds` does.

---

_Label `codex` removed by @konstin on 2025-07-21 09:40_

---

_Label `enhancement` added by @konstin on 2025-07-21 09:40_

---

_Renamed from "Always include the patch version in `uv_build` upper bounds" to "Match `--bounds` style in `uv_build` bounds in `uv init`" by @konstin on 2025-07-21 09:41_

---

_Renamed from "Match `--bounds` style in `uv_build` bounds in `uv init`" to "Match `--bounds` style for `uv_build` bounds in `uv init`" by @konstin on 2025-07-21 09:41_

---

_Renamed from "Match `--bounds` style for `uv_build` bounds in `uv init`" to "Match `--bounds` formatting for `uv_build` bounds in `uv init`" by @konstin on 2025-07-21 09:41_

---

_@konstin approved on 2025-07-21 09:42_

---

_Merged by @konstin on 2025-07-21 09:48_

---

_Closed by @konstin on 2025-07-21 09:48_

---

_Branch deleted on 2025-07-21 09:48_

---
