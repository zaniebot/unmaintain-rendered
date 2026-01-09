---
number: 9686
title: "Issue with uv backend to \"sync\""
type: issue
state: closed
author: franzhaas
labels:
  - question
  - preview
assignees: []
created_at: 2024-12-06T15:22:56Z
updated_at: 2024-12-06T17:04:38Z
url: https://github.com/astral-sh/uv/issues/9686
synced_at: 2026-01-07T13:12:18-06:00
---

# Issue with uv backend to "sync"

---

_Issue opened by @franzhaas on 2024-12-06 15:22_

Dear all,

![Image](https://github.com/user-attachments/assets/d67fc4de-cd24-4126-916a-7ea13ad75806)

There appears to be an issue with the uv backend when running "uv sync" in the directory of a library.

This surprised me as according to this https://github.com/astral-sh/uv/issues/8779 editable should already work...

Thanks,
Franz

---

_Label `preview` added by @zanieb on 2024-12-06 15:24_

---

_Assigned to @konstin by @zanieb on 2024-12-06 15:24_

---

_Comment by @konstin on 2024-12-06 15:43_

You need to set `UV_PREVIEW=1` as environment variable to use the uv build backend preview by itself, and use `--preview` with all uv commands that use package with the uv build backend.

---

_Closed by @konstin on 2024-12-06 15:43_

---

_Label `question` added by @konstin on 2024-12-06 15:43_

---

_Comment by @zanieb on 2024-12-06 15:47_

@konstin can we have a better error here that suggests that? Or is it not worth the effort?

---

_Comment by @konstin on 2024-12-06 16:19_

This is for early preview only and to avoid people from accidentally using the uv build backend before it's in a stable state, so it's not worth optimizing at this point.

---

_Comment by @zanieb on 2024-12-06 16:59_

Won't it be in preview for a while? I have a pretty strong preference for having a clearer error message. It wasn't clear to me that this wasn't a bug. Would you mind if I implemented something? 

---

_Comment by @konstin on 2024-12-06 17:04_

I don't mind adding a specific error message here.

---

_Referenced in [astral-sh/uv#9691](../../astral-sh/uv/pulls/9691.md) on 2024-12-06 17:23_

---
