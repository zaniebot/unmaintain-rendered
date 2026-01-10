---
number: 9114
title: "Q: how to run `python -m build` with uvx?"
type: issue
state: closed
author: dimaqq
labels:
  - question
assignees: []
created_at: 2024-11-14T04:56:03Z
updated_at: 2024-11-14T05:06:00Z
url: https://github.com/astral-sh/uv/issues/9114
synced_at: 2026-01-10T01:24:36Z
---

# Q: how to run `python -m build` with uvx?

---

_Issue opened by @dimaqq on 2024-11-14 04:56_

I just can't figure it out.

I was going to use `pip install build; python -m build` as a stepping stone towards converting the project to uv... so I figured I'd `uvx` it, and can't figure out the syntax... if it's possible at all.

---

_Comment by @charliermarsh on 2024-11-14 05:00_

Yeah, it's `uvx --from build pyproject-build`. 

---

_Comment by @charliermarsh on 2024-11-14 05:01_

If you run `uvx build`, it should point you to that:

```
❯ uvx build
Installed 3 packages in 7ms
The executable `build` was not found.
warning: An executable named `build` is not provided by package `build`.
The following executables are provided by `build`:
- pyproject-build
Consider using `uvx --from build <EXECUTABLE_NAME>` instead.
```

(The extra `--from` is necessary because `build` names its executable `pyproject-build`, not `build`.)

---

_Label `question` added by @charliermarsh on 2024-11-14 05:01_

---

_Comment by @dimaqq on 2024-11-14 05:06_

Ahh it doesn't I must've had a brain freeze and couldn't substitute EXECUTABLE_NAME correctly!

---

_Closed by @dimaqq on 2024-11-14 05:06_

---
