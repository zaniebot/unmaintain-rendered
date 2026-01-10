---
number: 7425
title: About remove venv and conda env list
type: issue
state: closed
author: WorkSync
labels:
  - question
assignees: []
created_at: 2024-09-16T13:23:03Z
updated_at: 2024-09-23T04:57:58Z
url: https://github.com/astral-sh/uv/issues/7425
synced_at: 2026-01-10T01:24:15Z
---

# About remove venv and conda env list

---

_Issue opened by @WorkSync on 2024-09-16 13:23_

Hello, I would like to ask if uv currently provides features for deleting virtual environments and viewing installed virtual environments, similar to the` conda env list` command in Anaconda. Thank you!

---

_Comment by @zanieb on 2024-09-22 23:44_

No we don't support this yet since we don't centrally manage environments — they're just created in working directories.

---

_Label `question` added by @zanieb on 2024-09-22 23:44_

---

_Comment by @WorkSync on 2024-09-23 04:57_

> No we don't support this yet since we don't centrally manage environments — they're just created in working directories.

Got it,sir.Have a nice day.

---

_Closed by @WorkSync on 2024-09-23 04:57_

---
