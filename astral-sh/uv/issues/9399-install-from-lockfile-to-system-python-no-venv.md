---
number: 9399
title: Install from lockfile to system python (no venv) (Containerized env).
type: issue
state: closed
author: g-bar
labels:
  - question
assignees: []
created_at: 2024-11-24T20:44:24Z
updated_at: 2024-11-30T20:15:21Z
url: https://github.com/astral-sh/uv/issues/9399
synced_at: 2026-01-10T01:24:40Z
---

# Install from lockfile to system python (no venv) (Containerized env).

---

_Issue opened by @g-bar on 2024-11-24 20:44_

I'm using uv to manage my dependencies in a Dockerfile.
Like so:

```
FROM python:3.12

ENV UV_SYSTEM_PYTHON=true
RUN pip install --no-cache-dir uv
COPY uv.lock pyproject.toml ./
RUN uv sync --frozen
```
I though the `UV_SYSTEM_PYTHON=true` sufficed but it's still creating a venv. Apparently the envvar and --system flag only apply to uv pip install, but that requires to specify the packages. I want to install from uv.lock.

I'm looking for something like poetry's `POETRY_VIRTUALENVS_CREATE` : https://python-poetry.org/docs/configuration#virtualenvscreate. 



---

_Comment by @FishAlchemist on 2024-11-24 22:53_

If you're going to install using ``uv sync`` into the system Python, you should consider setting the ``UV_PROJECT_ENVIRONMENT`` variable.
https://docs.astral.sh/uv/concepts/projects/config/#project-environment-path

---

_Label `question` added by @charliermarsh on 2024-11-25 00:56_

---

_Comment by @charliermarsh on 2024-11-25 00:56_

Yeah, you need to use `UV_PROJECT_ENVIRONMENT` here.

---

_Comment by @g-bar on 2024-11-30 20:15_

Thank you!

---

_Closed by @g-bar on 2024-11-30 20:15_

---
