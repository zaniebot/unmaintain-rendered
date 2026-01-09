---
number: 16819
title: requires-python in pyproject.toml is not (fully) taken into account
type: issue
state: closed
author: bohning
labels:
  - question
assignees: []
created_at: 2025-11-22T22:26:23Z
updated_at: 2025-11-23T17:06:27Z
url: https://github.com/astral-sh/uv/issues/16819
synced_at: 2026-01-07T13:12:19-06:00
---

# requires-python in pyproject.toml is not (fully) taken into account

---

_Issue opened by @bohning on 2025-11-22 22:26_

### Summary
I have 
```
[project]
...
requires-python = ">3.10,<3.13"
...
```
in my `pyproject.toml`. When I run `uv venv .venv`, I get Python 3.10.18, which is not what I expected (3.11, 3.12). Does uv really consider 3.10.18 > 3.10?

I can however manually state the required version via `uv venv .venv --python 3.12` (or 3.11), but of course that's not what's expected to be done.

Changing the line in `pyproject.toml` to `requires-python = ">=3.11,<3.13"` does indeed install Python 3.11 (so this could be considered a fix/workaround).

With this change, the `resolution-markers` in `uv.lock` disappear - however, they seemed contradictory:

<img width="290" height="124" alt="Image" src="https://github.com/user-attachments/assets/29a93ae2-ddc4-40cc-bea7-e3129a31baa8" />

### Platform

macOS Sequoia 15.5 (x86_64, Intel)

### Version

0.9.11

### Python version

Python 3.12.12 (3.11.14, 3.10.18)


---

_Label `bug` added by @bohning on 2025-11-22 22:26_

---

_Comment by @zanieb on 2025-11-22 23:30_

`>3.10` does mean `>3.10.0` per the Python requirement specification (PEP 440), yes.

---

_Label `bug` removed by @zanieb on 2025-11-22 23:30_

---

_Label `question` added by @zanieb on 2025-11-22 23:30_

---

_Comment by @bohning on 2025-11-23 17:04_

Thanks for the quick reply, that clears things up (though I still think that the `resolution-markers` in the screenshot above could hint to a bug, so feel free to close this as you prefer)!

---

_Comment by @zanieb on 2025-11-23 17:06_

Those markers are intended to be non-overlapping, they are forks in the resolution state that we track for internal purposes.

---

_Closed by @zanieb on 2025-11-23 17:06_

---
