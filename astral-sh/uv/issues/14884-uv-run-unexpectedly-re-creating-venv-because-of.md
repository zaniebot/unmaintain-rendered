---
number: 14884
title: "`uv run` unexpectedly re-creating venv because of `.python-version` created by `uv init`"
type: issue
state: closed
author: silverwind
labels:
  - bug
assignees: []
created_at: 2025-07-25T06:58:41Z
updated_at: 2025-07-27T14:30:01Z
url: https://github.com/astral-sh/uv/issues/14884
synced_at: 2026-01-10T01:25:50Z
---

# `uv run` unexpectedly re-creating venv because of `.python-version` created by `uv init`

---

_Issue opened by @silverwind on 2025-07-25 06:58_

### Summary

My goal is to create a venv with a specific python version and run it. In a fresh directory, I run:

```console
$ uv init
Initialized project `app`
$ uv sync -p 3.14
Using CPython 3.14.0b4
Creating virtual environment at: .venv
$ uv run python -V
Using CPython 3.12.3 interpreter at: /usr/bin/python3.12
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Python 3.12.3
```

Why does `uv run` re-create the venv? `pyproject.toml` has `requires-python = ">=3.12"` which 3.14 satisfies.

### Platform

Ubuntu 24.04

### Version

0.8.2

### Python version

3.12.3

---

_Label `bug` added by @silverwind on 2025-07-25 06:58_

---

_Renamed from "`uv run` unexpectedly re-creating `.venv`" to "`uv run` unexpectedly re-creating `.venv` created with specific python version" by @silverwind on 2025-07-25 07:02_

---

_Comment by @silverwind on 2025-07-25 07:11_

It seems the issue is because `uv init` created this file:

```console
$ cat .python-version
3.12
```

If I remove that file it works. Maybe it should be considered not creating this file.

---

_Comment by @SoundDesignerToBe on 2025-07-25 09:22_

I had `uv run --active ...` break my existing virtual environment, when my VPN was not working and it could not properly access packages. So it started to overwrite the existing environment and because I could not re-install at that time, I was left without a working virtual environment.

---

_Renamed from "`uv run` unexpectedly re-creating `.venv` created with specific python version" to "`uv run` unexpectedly re-creating virtualenv because of `.python-version` created by `uv init`" by @silverwind on 2025-07-25 09:54_

---

_Comment by @silverwind on 2025-07-25 09:55_

Above comment seems unrelated. I have updated the issue title to mention this is specifically related to `.python-version` created by `uv init`.

---

_Renamed from "`uv run` unexpectedly re-creating virtualenv because of `.python-version` created by `uv init`" to "`uv run` unexpectedly re-creating venv because of `.python-version` created by `uv init`" by @silverwind on 2025-07-25 09:57_

---

_Comment by @SoundDesignerToBe on 2025-07-25 16:13_

Should I file a separate issue?

---

_Comment by @zanieb on 2025-07-25 16:20_

If you want to change the Python version for a `uv run` invocation, you should include it in the command, e.g., `uv run -p 3.14`.

---

_Comment by @silverwind on 2025-07-27 09:11_

> If you want to change the Python version for a uv run invocation, you should include it in the command, e.g., uv run -p 3.14.

Yeah that works as well, but is impractical for some use cases like calling `uv run` in numerous places across a big repo.

So I think this is not a bug, but a odd interaction between `uv init` and `uv run`. I won't be using `uv init` myself anymore, as it does too much.

---

_Closed by @silverwind on 2025-07-27 09:11_

---

_Comment by @zanieb on 2025-07-27 14:30_

You can just use `uv init --no-pin-python`

---

_Referenced in [astral-sh/uv#16362](../../astral-sh/uv/issues/16362.md) on 2025-10-19 21:42_

---
