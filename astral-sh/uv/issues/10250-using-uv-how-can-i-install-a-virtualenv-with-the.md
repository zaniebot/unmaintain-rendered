```yaml
number: 10250
title: "Using `uv`, how can I install a virtualenv with the editable dependencies copied into the virtualenv instead of a .pth file?"
type: issue
state: closed
author: vf-jabadia
labels:
  - needs-mre
assignees: []
created_at: 2024-12-31T12:15:40Z
updated_at: 2024-12-31T15:51:52Z
url: https://github.com/astral-sh/uv/issues/10250
synced_at: 2026-01-12T16:00:09Z
```

# Using `uv`, how can I install a virtualenv with the editable dependencies copied into the virtualenv instead of a .pth file?

---

_@vf-jabadia_

My understanding from reading the docs is that `uv sync --no-editable` should copy the sources of editable dependencies into the `.venv` site-packages.

I'm trying to dockerize a project with a multi-stage build like this:

```Dockerfile
FROM python:3.12 AS dependencies

# install uv following https://docs.astral.sh/uv/guides/integration/docker/#installing-uv
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

ENV PYTHONUNBUFFERED=1
ENV PYTHONDONTWRITEBYTECODE=1

# install dependencies
ADD web/pyproject.toml web/uv.lock /app/web/
ADD shared /app/shared
WORKDIR /app/web
RUN uv sync --locked --no-dev --no-cache --no-editable -v  # here is the--no-editable
RUN ls -la /app/web/.venv/lib/python3.12/site-packages. # here we see the _shared.pth file
RUN cat /app/web/.venv/lib/python3.12/site-packages/_shared.pth. # it contains /app/shared

FROM python:3.12-slim
COPY --from=dependencies /app/web /app/web

# ... rest of Dockerfile here ...
# ... the resulting .venv contains.a `/app/web/.venv/lib/python3.12/site-packages/_shared.pth` instead of the sources ...

```

`pyproject.toml` looks like this:

```toml
[project]
name = "web"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "celery>=5.4.0",
    "django>=5.1.4",
    "psycopg>=3.2.3",
    "shared",
]

[dependency-groups]
dev = [
    "flake8>=7.1.1",
    "flake8-pyproject>=1.2.3",
    "pytest>=8.3.4",
    "pytest-django>=4.9.0",
]

[tool.uv.sources]
shared = { path = "../shared", editable = true }
```

Is it a bug? Or is it that I'm not understanding the --no-editable behaviour properly?

Thanks

---

_Comment by @charliermarsh on 2024-12-31 14:37_

Can you create a Git repository that I can clone + a set of commands to run, to reproduce?

---

_Label `needs-mre` added by @charliermarsh on 2024-12-31 14:37_

---

_Comment by @vf-jabadia on 2024-12-31 15:50_

my mistake, sorry for the inconvenience

my original `Dockerfile` read like this:

```Dockerfile
...
RUN uv sync --locked --no-dev --no-cache --no-editable -v  # here is the--no-editable
RUN uv add gunicorn
...
```
The `uv add` command was reverting the `--no-editable` effect. I found it when creating the minimal repo to reproduce.

Thanks for the quick response and sorry about that

---

_Closed by @vf-jabadia on 2024-12-31 15:50_

---
