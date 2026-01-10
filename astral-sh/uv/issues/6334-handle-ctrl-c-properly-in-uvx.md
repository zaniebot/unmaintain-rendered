```yaml
number: 6334
title: "Handle Ctrl-C properly in `uvx`"
type: issue
state: closed
author: zanieb
labels:
  - bug
assignees: []
created_at: 2024-08-21T15:00:44Z
updated_at: 2024-08-21T17:04:19Z
url: https://github.com/astral-sh/uv/issues/6334
synced_at: 2026-01-10T04:45:09Z
```

# Handle Ctrl-C properly in `uvx`

---

_Issue opened by @zanieb on 2024-08-21 15:00_

As we do in `uv run`

---

_Label `bug` added by @zanieb on 2024-08-21 15:00_

---

_Comment by @charliermarsh on 2024-08-21 15:02_

The setup is identical for both commands, I think...? What are you seeing.

---

_Comment by @zanieb on 2024-08-21 15:04_

Oh. Mentioned at https://github.com/astral-sh/uv/issues/6329#issuecomment-2302211339 cc @bluss 

---

_Comment by @charliermarsh on 2024-08-21 15:04_

Thanks I'll check it out. Maybe has to do with the wrapper? Not sure yet.

---

_Comment by @bluss on 2024-08-21 15:11_

> Thanks I'll check it out. Maybe has to do with the wrapper? Not sure yet.

That's a surprise, but it's the case, the problem is for uvx and not uv tool run.

---

_Comment by @zanieb on 2024-08-21 15:15_

Oh, we invoke `uv tool run` as a subprocess in `uvx`. We'll need to be more sophisticated there.

---

_Assigned to @charliermarsh by @zanieb on 2024-08-21 15:18_

---

_Comment by @charliermarsh on 2024-08-21 15:23_

👍 👍 

---

_Comment by @prrao87 on 2024-08-21 15:45_

Hang on, is `uvx` fundamentally different from `uv tool run` in the sense that it's invoked as a subprocess?
The docs page here seems to indicate that it's simply an alias:
https://docs.astral.sh/uv/guides/tools/#running-tools

I'm now thoroughly confused when I should be using `uvx` vs. `uv tool run` 😅.

---

_Comment by @charliermarsh on 2024-08-21 15:50_

They're meant to be entirely equivalent. It's an "alias" but it's not literally a bash alias (since we don't have a way to "install" an alias), it's a tiny wrapper script that just forwards on to `uv tool run`.

---

_Comment by @charliermarsh on 2024-08-21 15:51_

Any perceptible or behavioral difference between `uvx` and `uv tool run` should be considered a bug.

---

_Comment by @charliermarsh on 2024-08-21 15:55_

(...like this one :joy:)

---

_Comment by @zanieb on 2024-08-21 15:57_

There's a few additional ms of overhead — but yeah it's functionally an alias just can't trivially be implemented as a true shell alias. There are a few other minor differences, like we change the help menu to say `uvx` when you're using it.

---

_Comment by @prrao87 on 2024-08-21 15:59_

I suppose what's confusing in the docs is that the top half of the tools docs page uses `uvx` and the bottom half switches to using `uv tool run`. I wonder if there's a clearer way to state that users can treat them as functionally equivalent, and have example commands that are consistent? 
https://docs.astral.sh/uv/guides/tools/

---

_Comment by @zanieb on 2024-08-21 16:12_

@prrao87 I'm a bit confused, I don't think we switch to `uv tool run`, we switch to `uv tool install` which is different.

---

_Comment by @charliermarsh on 2024-08-21 16:44_

Closed by #6346.

---

_Closed by @charliermarsh on 2024-08-21 16:44_

---

_Comment by @prrao87 on 2024-08-21 17:04_

@zanieb I now understand it much better. Thanks for clarifying!

---
