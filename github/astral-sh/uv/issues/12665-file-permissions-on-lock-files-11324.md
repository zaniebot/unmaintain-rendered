---
number: 12665
title: "File permissions on lock files (#11324)"
type: issue
state: open
author: Lalufu
labels:
  - question
assignees: []
created_at: 2025-04-04T09:29:19Z
updated_at: 2025-04-04T09:29:19Z
url: https://github.com/astral-sh/uv/issues/12665
synced_at: 2026-01-07T13:12:18-06:00
---

# File permissions on lock files (#11324)

---

_Issue opened by @Lalufu on 2025-04-04 09:29_

### Question

In #11324 (addressed in #11328) there was a complaint about a lock file error in case a `uv` environment was created by one user, but then used by another. The fix for this was to create the lock files in question with `0o777` permissions, ignoring any user `umask`. While this fixes the problem, it now leaves world writable files on the file system, which isn't great practice, either.
What I'm not understanding from the original issue, and what I'd like some clarification on, are the consequences of not being able to acquire the lock file in question. If this lock file is taken to prevent concurrent changes to other files, in the case presented this would not be possible in any case, as all other files are also owned by a different user? What does taking the lock file guard? What could happen if `uv run` continued without having taken the lock in this case?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @Lalufu on 2025-04-04 09:29_

---

_Referenced in [astral-sh/uv#16769](../../astral-sh/uv/issues/16769.md) on 2025-11-18 16:56_

---
