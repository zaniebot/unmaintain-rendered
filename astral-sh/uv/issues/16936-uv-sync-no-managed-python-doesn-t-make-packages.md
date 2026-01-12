```yaml
number: 16936
title: "uv sync --no-managed-python doesn't make packages available to system Python interpreter"
type: issue
state: closed
author: SnoozeFreddo
labels:
  - question
assignees: []
created_at: 2025-12-02T18:28:39Z
updated_at: 2025-12-02T20:23:36Z
url: https://github.com/astral-sh/uv/issues/16936
synced_at: 2026-01-12T16:02:40Z
```

# uv sync --no-managed-python doesn't make packages available to system Python interpreter

---

_@SnoozeFreddo_

When using uv sync --no-managed-python in a Docker container, installed packages are not accessible to the system Python interpreter (/usr/local/bin/python3), resulting in ModuleNotFoundError when trying to run scripts directly with Python.

Context
I'm building a Docker image for a FastAPI application where I need the installed packages to be available to the system Python interpreter for IDE remote debugging (IntelliJ PyCharm remote interpreter). While uv run works correctly, it's not compatible with IDE remote interpreter debugging workflows since IntelliJ doesn't offer the option to execute uv inside the container, only locally.

Minimal Container:

```
FROM python:3.12.12-slim-trixie AS base
COPY --from=ghcr.io/astral-sh/uv:0.9.13 /uv /uvx /bin/

WORKDIR /someproj
ENV UV_LINK_MODE=copy

ENV UV_COMPILE_BYTECODE=1

# Install dependencies
COPY pyproject.toml uv.lock ./
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --locked --no-install-project --no-managed-python

 # Fastapi
ENTRYPOINT ["python3", "-m", "uvicorn", "someproj:app"]
```



Running `/usr/local/bin/python3 -m uvicorn` results in:

/usr/local/bin/python3: No module named uvicorn


What's the recommended approach for Docker containers where packages need to be accessible to the system Python interpreter for IDE debugging purposes? Did i get the whole thing wrong?

How should i start "uv run" in order to use the system interpreter without installing the packages again,

### Platform

Debian Trixie

### Version

0.9.13

---

_Label `question` added by @SnoozeFreddo on 2025-12-02 18:28_

---

_Comment by @zanieb on 2025-12-02 19:21_

You'll need to set `UV_PROJECT_ENVIRONMENT` to the system Python environment instead. Otherwise, `uv sync` will always create a virtual environment (and `--no-managed-python` just uses the system Python interpreter to do so)

---

_Comment by @SnoozeFreddo on 2025-12-02 19:50_

@zanieb I see, thanks. Got confused because there was no .venv folder when using --no-managed-python in the current dir, compared to when executed without the flag. UV_PROJECT_ENVIRONMENT doesn't work well either, because Debian splits the interpreter and packages into two folders.

> Project virtual environment directory `/usr/local/lib/python3.12` cannot be used because it is not a valid Python environment (no Python executable was found)

Is what i get





Tried to add the --python flag to tell it where it is but it didn't solve the issue.


---

_Comment by @zanieb on 2025-12-02 19:55_

I think you just want `UV_PROJECT_ENVIRONMENT=/usr/local`?

---

_Comment by @SnoozeFreddo on 2025-12-02 20:23_

Yep, this worked fine. It seems like i also don't need to use UV_NO_MANAGED_PYTHON=1 any more. Thank you!

---

_Closed by @SnoozeFreddo on 2025-12-02 20:23_

---
