```yaml
number: 3061
title: Use standalone Python builds when creating a virtual environment
type: issue
state: closed
author: pfmoore
labels:
  - duplicate
  - enhancement
assignees: []
created_at: 2024-04-16T11:19:36Z
updated_at: 2024-04-19T14:11:57Z
url: https://github.com/astral-sh/uv/issues/3061
synced_at: 2026-01-12T15:58:41Z
```

# Use standalone Python builds when creating a virtual environment

---

_@pfmoore_

When trying to reproduce an issue on a Python version that I don't have installed, it would be useful to be able to create a virtual environment for that version *without* needing to install a system copy of the Python version first. Other tools like hatch and PDM use the [Python standalone builds](https://github.com/indygreg/python-build-standalone) project for this. Clearly I could download the appropriate interpreter, unpack it, and run `uv venv --python /path/to/unpacked/build`, but that would mean manually managing the unpacked version.

Being able to do something like `uv venv --python pypy@3.10`, and have `uv` download, unpack and cache the interpreter for me would be useful in this situation.

I should note that this isn't a common occurrence for me, so I'm not asking for this feature to be prioritised. The workarounds I currently have are perfectly fine. But this does feel to me like a natural extension to the current `uv venv` feature set.

---

_Assigned to @zanieb by @zanieb on 2024-04-16 13:09_

---

_Comment by @zanieb on 2024-04-16 13:09_

We'll support this.

---

_Label `enhancement` added by @zanieb on 2024-04-16 13:10_

---

_Comment by @zanieb on 2024-04-19 14:11_

Closing in favor of https://github.com/astral-sh/uv/issues/3061

---

_Closed by @zanieb on 2024-04-19 14:11_

---

_Label `duplicate` added by @zanieb on 2024-04-19 14:11_

---
