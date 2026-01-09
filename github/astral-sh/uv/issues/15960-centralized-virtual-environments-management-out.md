---
number: 15960
title: centralized virtual environments management out of project like poetry
type: issue
state: closed
author: DuroyGeorge
labels:
  - enhancement
assignees: []
created_at: 2025-09-20T15:06:37Z
updated_at: 2025-09-20T16:17:16Z
url: https://github.com/astral-sh/uv/issues/15960
synced_at: 2026-01-07T13:12:19-06:00
---

# centralized virtual environments management out of project like poetry

---

_Issue opened by @DuroyGeorge on 2025-09-20 15:06_

### Summary

The default behavior of `uv venv` is creating an virtual environment in the current working diretory.If I add next param like `~/.venvs` which is troublesome, the vscode python extenion can't recongnize the environment outside the working directory automatically.


### Example

So I wish uv could be configured to behave like poetry which can create virtual environment outside the working directory and make it recongnized by vscode python extention which will save lots of time activating the venv in a new terminal againg and againg.

---

_Label `enhancement` added by @DuroyGeorge on 2025-09-20 15:06_

---

_Comment by @zanieb on 2025-09-20 16:16_

Please read https://github.com/astral-sh/uv/issues/9452

This is a duplicate of https://github.com/astral-sh/uv/issues/1495, though I think your problem goes away if you instead use `uv sync` in projects since that's intended to create a virtual environment in the project root for discovery with IDEs.

You may also be interested in #15787

---

_Closed by @zanieb on 2025-09-20 16:17_

---
