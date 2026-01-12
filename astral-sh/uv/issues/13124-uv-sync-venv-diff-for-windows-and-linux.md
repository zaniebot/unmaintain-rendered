```yaml
number: 13124
title: uv sync venv diff for windows and linux
type: issue
state: closed
author: paul-yangmy
labels:
  - question
assignees: []
created_at: 2025-04-27T03:27:30Z
updated_at: 2025-04-28T02:05:18Z
url: https://github.com/astral-sh/uv/issues/13124
synced_at: 2026-01-12T16:01:20Z
```

# uv sync venv diff for windows and linux

---

_@paul-yangmy_

### Question

When using uv init to initialize a project on Windows, it creates a Scripts directory under .venv, while on Linux it creates a bin directory. This caused an error when I deployed a program written on Windows to a Linux server:

error: Project virtual environment directory xxx/.venv cannot be used because it is not a valid Python environment (no Python executable was found)

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @paul-yangmy on 2025-04-27 03:27_

---

_Comment by @paul-yangmy on 2025-04-27 03:27_

thanks!

---

_Comment by @FishAlchemist on 2025-04-27 12:25_

Yep, you'll run into the same issue even using Python's venv itself. That's expected.
Why would you need cross-platform virtual environments? Generally, the use case is to run ``uv sync`` again in the new environment, rather than simply taking the virtual environment with you.

---

_Comment by @charliermarsh on 2025-04-27 15:08_

👍 You generally can't take a `.venv` created on one machine and use it on another that's running a different operating system or architecture. You should re-run whatever commands you ran to create the environment, over on the new machine.

---

_Closed by @charliermarsh on 2025-04-27 15:08_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-27 15:08_

---

_Comment by @paul-yangmy on 2025-04-28 02:05_

Oh thanks!

---
