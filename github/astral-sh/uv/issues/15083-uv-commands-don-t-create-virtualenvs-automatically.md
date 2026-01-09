---
number: 15083
title: "UV commands don't create VirtualEnvs automatically"
type: issue
state: open
author: Lyranile
labels:
  - question
assignees: []
created_at: 2025-08-05T14:35:46Z
updated_at: 2025-10-27T19:02:16Z
url: https://github.com/astral-sh/uv/issues/15083
synced_at: 2026-01-07T13:12:19-06:00
---

# UV commands don't create VirtualEnvs automatically

---

_Issue opened by @Lyranile on 2025-08-05 14:35_

As an example when I make a new project
```
uv init
error: No interpreter found at path `.venv/bin/python`
```
I get no interpreter found at path, which I get that i need to do `uv venv --python 3.13`
But I was expecting it to make this automatically
This is similar to uv sync and any other command
Also, when moving between projects, it might use the .venv located at my other project

On another topic, when making the virtualenv, I always have to do `source .venv/bin/python`
Doesn't it make sense for the uv venv to automatically activate it?

---

_Comment by @zanieb on 2025-08-05 16:25_

Please share verbose logs. I don't see why `uv init` would fail with that message.

> Doesn't it make sense for the uv venv to automatically activate it?

We cannot do this without writing an integration for each shell. This is an intentional limitation in the design of shells. See #1910 

---

_Label `question` added by @zanieb on 2025-08-05 16:25_

---

_Comment by @jakob1379 on 2025-08-22 08:31_

This could happen if he has `UV_PYTHON_PREFERENCE=system` or similarly `UV_PYHON=python` where `python` points to a system installed python and not managed by `uv`

---

_Comment by @heavenyoung1 on 2025-10-25 21:37_

Currently, uv does not provide a built-in command to activate a project's Python virtual environment. This functionality would enhance the user experience by allowing developers to quickly enter the virtual environment associated with their project.

**Proposed Solution:**

Introduce a command such as `uv activate` that activates the project's Python virtual environment. This command would serve as a shorthand for sourcing the appropriate activation script, depending on the operating system:

---

_Comment by @konstin on 2025-10-27 19:02_

@heavenyoung1 You're looking for https://github.com/astral-sh/uv/issues/1910

---
