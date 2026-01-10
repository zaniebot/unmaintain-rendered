---
number: 13269
title: "uv fails when Poetry pyproject.toml defines version under [tool.poetry] instead of [project]"
type: issue
state: closed
author: nhubbard
labels:
  - question
assignees: []
created_at: 2025-05-02T15:54:16Z
updated_at: 2025-05-02T16:47:50Z
url: https://github.com/astral-sh/uv/issues/13269
synced_at: 2026-01-10T01:25:31Z
---

# uv fails when Poetry pyproject.toml defines version under [tool.poetry] instead of [project]

---

_Issue opened by @nhubbard on 2025-05-02 15:54_

### Summary

I'm trying to install the `label-studio-sdk` package from Git using UV. It fails with the following error:

```
0.462 Using Python 3.12.10 environment at: /venv
0.788    Updating https://github.com/HumanSignal/label-studio-ml-backend.git (HEAD)
5.466     Updated https://github.com/HumanSignal/label-studio-ml-backend.git (be9809a2e279f266bad38eca28ae3543237d8c47)
6.842    Updating https://github.com/HumanSignal/label-studio-sdk.git (HEAD)
11.47     Updated https://github.com/HumanSignal/label-studio-sdk.git (474abc4bc560744a5e44435a9971fcdb08e62d64)
11.47   × Failed to download and build `label-studio-sdk @
11.47   │ git+https://github.com/HumanSignal/label-studio-sdk.git`
11.47   ├─▶ Failed to parse:
11.47   │   `/.cache/git-v0/checkouts/73f42ed57643abd9/474abc4/pyproject.toml`
11.47   ╰─▶ TOML parse error at line 1, column 1
11.47         |
11.47       1 | [project]
11.47         | ^^^^^^^^^
11.47       `pyproject.toml` is using the `[project]` table, but the required
11.47       `project.version` field is neither set nor present in the
11.47       `project.dynamic` list
11.47
------
target backend: failed to solve: process "/bin/sh -c uv pip install -r requirements.txt" did not complete successfully: exit code: 1
```

Is this an error caused by an actually invalid pyproject.toml file, or is UV failing because Poetry specifies the version in a different place?

### Platform

Docker under `ghcr.io/astral-sh/uv:python3.12-bookwork-slim` container running on x86_64 machine under Arch Linux

### Version

The latest version from the container

### Python version

Latest 3.12 release under the container

---

_Label `bug` added by @nhubbard on 2025-05-02 15:54_

---

_Label `bug` removed by @konstin on 2025-05-02 16:04_

---

_Label `question` added by @konstin on 2025-05-02 16:04_

---

_Comment by @konstin on 2025-05-02 16:04_

The `pyproject.toml` is actually invalid.

`[tool.poetry]` is an old pre-standard format that only poetry uses. To ensure compatibility with the standard (PEP 621), you need to either migrate to `[project]` (with poetry 2.0), or remove the `[project]` table and rely on the poetry build backend. A `[project]` table with only a build backend however is invalid.

---

_Comment by @nhubbard on 2025-05-02 16:47_

Good to know, thanks. I'll open an issue on their bug tracker.

---

_Closed by @nhubbard on 2025-05-02 16:47_

---

_Referenced in [HumanSignal/label-studio-sdk#449](../../HumanSignal/label-studio-sdk/issues/449.md) on 2025-05-02 16:49_

---

_Referenced in [fern-api/fern#9617](../../fern-api/fern/issues/9617.md) on 2025-09-25 19:42_

---
