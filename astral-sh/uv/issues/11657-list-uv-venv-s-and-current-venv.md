```yaml
number: 11657
title: "List UV venv's and current venv"
type: issue
state: closed
author: oliverangelil
labels:
  - question
assignees: []
created_at: 2025-02-20T08:00:25Z
updated_at: 2025-02-20T14:29:44Z
url: https://github.com/astral-sh/uv/issues/11657
synced_at: 2026-01-12T16:00:43Z
```

# List UV venv's and current venv

---

_@oliverangelil_

### Question

How can I list all the available uv venvs and also show the venv currently activated?

### Platform

Debian GNU/Linux 12 (bookworm)

### Version

uv 0.6.2

---

_Label `question` added by @oliverangelil on 2025-02-20 08:00_

---

_Comment by @konstin on 2025-02-20 08:39_

uv does not keep track of the created venvs, so it can't tell you which venvs exist. The currently activate venv should be shown in your terminal promt, otherwise `which python` will point your to the active Python interpreter.

---

_Closed by @oliverangelil on 2025-02-20 14:19_

---

_Comment by @zanieb on 2025-02-20 14:29_

You can also use `uv python find` to find the interpreter uv would use.

---
