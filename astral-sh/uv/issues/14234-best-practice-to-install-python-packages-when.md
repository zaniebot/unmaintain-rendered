```yaml
number: 14234
title: Best practice to install Python packages when developing on container with watch mode
type: issue
state: open
author: jsulopzs
labels:
  - question
assignees: []
created_at: 2025-06-24T09:25:16Z
updated_at: 2025-06-24T09:39:08Z
url: https://github.com/astral-sh/uv/issues/14234
synced_at: 2026-01-12T16:01:45Z
```

# Best practice to install Python packages when developing on container with watch mode

---

_@jsulopzs_

### Question

**Issue Summary**

When adding new packages to `pyproject.toml` and rebuilding the Docker container, the new library (e.g., `seaborn`) is not found at runtime:
`ModuleNotFoundError: No module named 'seaborn'`.

However, when running `uv add seaborn` locally, it updates `uv.lock` and creates a `.venv`, which is redundant in this Docker-based workflow since the container builds its own isolated environment.

**Expected Behavior**

I'd like to:

* Add a dependency to `pyproject.toml`.
* Rebuild the container.
* Have `uv.lock` reflect the new dependency.
* Avoid creating a `.venv` locally, since the container will manage its own environment.

**Repro Details**

`pyproject.toml`:

```toml
[project]
name = "esios-client"
version = "0.1.0"
dependencies = [
    ...
    "seaborn",
]

[project.optional-dependencies]
dev = ["ruff", "pytest"]

[tool.hatch.build.targets.wheel]
packages = ["src"]

[tool.ruff]
line-length = 100

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

Entry point:

```python
import seaborn as sns
import streamlit as st
st.write('whatever')
```

Dockerfile:

```Dockerfile
FROM ghcr.io/astral-sh/uv:python3.13-bookworm-slim AS builder
ENV UV_COMPILE_BYTECODE=1 UV_LINK_MODE=copy

ENV UV_PYTHON_DOWNLOADS=0

ARG ENV
ENV ENV=${ENV}

# Set working directory
WORKDIR /app

# Install dependencies
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    if [ "${ENV}" = "production" ]; then \
    uv sync --frozen --no-install-project --no-dev --no-editable; \
    elif [ "${ENV}" = "development" ]; then \
    uv sync --frozen --no-install-project --no-dev; \
    fi

# Copy the project into the intermediate image
ADD . /app

# Sync the project with bytecode compilation
ENV UV_COMPILE_BYTECODE=1
RUN --mount=type=cache,target=/root/.cache/uv \
    if [ "${ENV}" = "production" ]; then \
    uv sync --frozen --no-dev --no-editable; \
    elif [ "${ENV}" = "development" ]; then \
    uv sync --frozen --no-dev; \
    fi

# Final stage
FROM python:3.13-slim-bookworm

# Copy the application from the builder
COPY --from=builder --chown=app:app /app /app

# Place executables in the environment at the front of the path
ENV PATH="/app/.venv/bin:$PATH"

# Set working directory to the frontend app
WORKDIR /app/src/frontend

# Expose the port Streamlit runs on
EXPOSE 8501

# Command to run the Streamlit app
CMD ["streamlit", "run", "main.py", "--server.port=8501", "--server.address=0.0.0.0"] 
```

**What works but causes a side effect**

Using `uv add seaborn` locally:

* Adds `seaborn` to `uv.lock`.
* Creates `.venv`, which is not used by the container.

**Question**

What’s the best way to update `uv.lock` (for Docker builds) when adding a new dependency—**without creating a local `.venv`**?

Docs reviewed: [https://docs.astral.sh/uv](https://docs.astral.sh/uv)

### Platform

Linux 6.10.14-linuxkit aarch64 GNU/Linux

### Version

Can't access uv inside the container, I'm using: FROM ghcr.io/astral-sh/uv:python3.13-bookworm-slim AS builder

---

_Label `question` added by @jsulopzs on 2025-06-24 09:25_

---

_Comment by @konstin on 2025-06-24 09:31_

Does
```
uv lock
```
work for this? It updates the lockfile and doesn't create a venv.

---

_Comment by @jsulopzs on 2025-06-24 09:38_

Yes, thank you. Also, is it possible to have it done automatically based on file change? I forgot to attach the compose:

```yml
services:
  frontend:
    build:
      context: ..
      dockerfile: docker/Dockerfile
      args:
        ENV: development
    ports:
      - "8501:8501"
    env_file:
      - .env
      - .env.dev
    develop:
      watch:
        # Sync the working directory with the `/app` directory in the container
        - action: sync
          path: ../
          target: /app
          ignore:
            - .venv/

        # Rebuild the image on changes to the pyproject.toml
        - action: rebuild
          path: ../pyproject.toml
        - action: rebuild
          path: ../uv.lock
    restart: unless-stopped
```



---
