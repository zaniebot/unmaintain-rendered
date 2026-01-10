---
number: 13830
title: uv sync deletes venv and creates a new one
type: issue
state: closed
author: aranvir
labels:
  - question
assignees: []
created_at: 2025-06-04T07:15:52Z
updated_at: 2025-06-04T14:58:37Z
url: https://github.com/astral-sh/uv/issues/13830
synced_at: 2026-01-10T01:25:39Z
---

# uv sync deletes venv and creates a new one

---

_Issue opened by @aranvir on 2025-06-04 07:15_

### Summary

Hi, I was surprised by the `uv sync --frozen` behavior just now.

* Cloned the project (python requirement >= 3.11)
* Created a .venv with Python 3.11.2 and seeded it
* Activated the .venv
* Used `uv sync --frozen` to sync the dependencies

And then uv downloaded Python 3.12 (although it was not needed or requested), removed the existing virtual environment, and created a new one. Even when using the `--active` flag ("Sync dependencies to the active virtual environment"), it behaves like this.

I haven't experienced this behavior in previous uv versions and it also feels "undocumented". The expected behavior is:
* Check for an active environment and use that
* Check for an existing environment in the project folder and use that
* Create a new environment only if there is no active or existing one

UV should not just delete existing environments without at least prompting the user to confirm it (or having the user submit a flag that implies confirmation).

Log lines:
```
> uv sync --frozen
Using CPython 3.12.10
Removed virtual environment at: .venv
Creating virtual environment at: .venv
...
```

### Platform

Debian 12

### Version

uv 0.7.10

### Python version

_No response_

---

_Label `bug` added by @aranvir on 2025-06-04 07:15_

---

_Comment by @konstin on 2025-06-04 07:48_

Can you share the verbose logs for the `uv sync -v` that recreates the environment?

---

_Comment by @zanieb on 2025-06-04 14:39_

> And then uv downloaded Python 3.12 (although it was not needed or requested)

It sounds like you have a `.python-version` file?

---

_Label `bug` removed by @zanieb on 2025-06-04 14:40_

---

_Label `question` added by @zanieb on 2025-06-04 14:40_

---

_Comment by @aranvir on 2025-06-04 14:58_

> > And then uv downloaded Python 3.12 (although it was not needed or requested)
> 
> It sounds like you have a `.python-version` file?

Yep that's it - I'm stupid. Thanks!

---

_Closed by @aranvir on 2025-06-04 14:58_

---

_Referenced in [astral-sh/uv#14031](../../astral-sh/uv/issues/14031.md) on 2025-06-13 16:32_

---
