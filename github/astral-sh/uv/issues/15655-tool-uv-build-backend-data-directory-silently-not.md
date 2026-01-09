---
number: 15655
title: "[tool.uv.build-backend.data] directory silently not included"
type: issue
state: closed
author: BartSchuurmans
labels:
  - question
  - build-backend
assignees: []
created_at: 2025-09-03T12:04:29Z
updated_at: 2025-09-08T15:02:30Z
url: https://github.com/astral-sh/uv/issues/15655
synced_at: 2026-01-07T13:12:19-06:00
---

# [tool.uv.build-backend.data] directory silently not included

---

_Issue opened by @BartSchuurmans on 2025-09-03 12:04_

### Summary

I'm trying to include a `.streamlit` directory in my source distribution and wheel. It lives next to my `pyproject.toml`.

Partial `pyproject.toml`:

```toml
[tool.uv]
package = true

[tool.uv.build-backend.data]
data = ".streamlit"
```

However, when running `uv build`, this directory does not appear in the output, and is not included in the source distribution or wheel.

Am I misunderstanding the documentation or is this a bug?

Documentation references:
- https://docs.astral.sh/uv/concepts/build-backend/#file-inclusion-and-exclusion
- https://docs.astral.sh/uv/reference/settings/#build-backend_data

### Platform

Darwin 24.6.0 arm64

### Version

uv 0.8.14 (Homebrew 2025-08-28)

### Python version

Python 3.12.6

---

_Label `bug` added by @BartSchuurmans on 2025-09-03 12:04_

---

_Assigned to @konstin by @konstin on 2025-09-03 12:13_

---

_Label `build-backend` added by @konstin on 2025-09-03 12:13_

---

_Label `bug` removed by @konstin on 2025-09-08 13:56_

---

_Label `needs-mre` added by @konstin on 2025-09-08 13:56_

---

_Comment by @konstin on 2025-09-08 13:56_

I can't reproduce, the example below includes the directory for me, can you share a minimal reproducible example?

```toml
[project]
name = "debug"
version = "0.1.0"
requires-python = ">=3.12.0"

[build-system]
requires = ["uv_build>=0.8.15,<0.9.0"]
build-backend = "uv_build"

[tool.uv.build-backend.data]
data = ".streamlit"
```

---

_Unassigned @konstin by @konstin on 2025-09-08 13:56_

---

_Comment by @BartSchuurmans on 2025-09-08 14:11_

@konstin Your example works correctly here; the difference is the inclusion of the `[build-system]` table. My `pyproject.toml` does not specify a build system, and I thought uv would then default to "uv's build backend", but the build output is clearly different between my and your example.

Which build backend is used when you don't specify one? Should I always specify one? Does `[tool.uv.build-backend.data]` only apply to a specifiy backend?

---

_Comment by @konstin on 2025-09-08 14:13_

By default, setuptools is used. That's fallback encoded in PEP 517 for the time before `pyproject.toml` `[build-system]` tables, so projects with a `setup.py` continue to work. To use the uv build backend, you need add a `[build-system]` (https://docs.astral.sh/uv/concepts/build-backend/#using-the-uv-build-backend).

We recommend always specifying a build backend.

---

_Label `needs-mre` removed by @konstin on 2025-09-08 14:14_

---

_Label `question` added by @konstin on 2025-09-08 14:14_

---

_Comment by @BartSchuurmans on 2025-09-08 14:17_

Thanks for your help @konstin!

---

_Closed by @BartSchuurmans on 2025-09-08 15:02_

---

_Referenced in [astral-sh/uv#15740](../../astral-sh/uv/issues/15740.md) on 2025-09-08 17:44_

---

_Referenced in [astral-sh/uv#15750](../../astral-sh/uv/pulls/15750.md) on 2025-09-09 12:23_

---

_Referenced in [astral-sh/uv#15765](../../astral-sh/uv/issues/15765.md) on 2025-09-10 08:45_

---
