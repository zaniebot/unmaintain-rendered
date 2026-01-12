```yaml
number: 14013
title: Memory usage increases when switching from --all-packages to --frozen --no-dev --no-cache in Docker
type: issue
state: closed
author: timothy-jeong
labels:
  - question
assignees: []
created_at: 2025-06-13T02:23:44Z
updated_at: 2025-06-25T02:20:42Z
url: https://github.com/astral-sh/uv/issues/14013
synced_at: 2026-01-12T16:01:41Z
```

# Memory usage increases when switching from --all-packages to --frozen --no-dev --no-cache in Docker

---

_@timothy-jeong_

### Question

>First of all, thank you for creating such an awesome package management tool. `uv` has made our Python monorepo setup significantly faster and easier to manage. We truly appreciate the effort behind this project!

Hi there 👋 I'm using Python to build a backend server, and we recently adopted uv as our package manager.

We're currently using uv to manage a **monorepo** that contains both shared modules and multiple microservices.

Here's a simplified version of our Dockerfile. We recently changed the uv sync command from `--all-packages`to `--frozen --no-dev --no-cache` for a leaner production build

```Dockerfile
FROM {some-python3.12-slim-arm64-image}
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/uv

WORKDIR /app

COPY pyproject.toml /app/pyproject.toml        # Project root
COPY uv.lock /app/uv.lock                      # Single uv.lock at root
COPY packages /app/packages                    # Shared modules
COPY services/a_service /app/services/a_service  # One microservice

# Previously used for convenience:
# RUN uv sync --all-packages
RUN uv sync --frozen --no-dev --no-cache

WORKDIR /app/services/a_service

ENV PYTHONPATH=/app:/app/services/a_service

RUN chmod +x start_servers.sh
EXPOSE {port}
CMD ["/bin/bash", "./start_servers.sh"]
```
However, after deploying the Docker container on AWS ECS (Fargate), I noticed that the container consumes ~5–7 percentage points more memory than it did with `--all-packages` option

Is there any known reason why `--frozen --no-dev --no-cache` option might lead to higher memory usage?

**For more context, I tested on macOS Environment when I add `--frozen` option to `uv sync` it increase Docker Container memory usage approximately  100MB**

Any insights or debugging suggestions would be greatly appreciated!

Thanks again 🙌

### Platform

macOS 15.5, arm64

### Version

uv 0.6.9 (3d9460278 2025-03-20)

---

_Label `question` added by @timothy-jeong on 2025-06-13 02:23_

---

_Comment by @zanieb on 2025-06-13 13:01_

`uv sync --all-packages` would ensure that your lockfile is up-to-date with your `pyproject.toml` first, while `uv sync --frozen --no-dev --no-cache` will just install directly from the lockfile, so your dependencies could be changing?

What about `uv sync --all-packages --frozen`? Do you see changes there?

---

_Comment by @timothy-jeong on 2025-06-14 04:14_

Hi @zanieb,
Thanks for your response!

As you suggested, I tried using `uv sync --all-packages --frozen`, and it slightly reduced memory usage.
From my repeated tests in a macOS local environment, it seems the `--frozen` option has a significant impact on memory usage.

The packages I'm testing include some local modules that are added as workspaces, but they don’t have fixed versions. Here's a simplified example:

```toml
[project]
name = "ABC-service"
version = "0.1.0"
description = "ABC!"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    ...
    "shared.a",
    "shared.b",
    "shared.c",
    "shared.d",
    "shared.e",
    "grpc_client.a",
    "grpc_client.b",
]

[tool.uv.sources]
"shared.a" = { workspace = true }
"shared.b" = { workspace = true }
"shared.c" = { workspace = true }
"shared.d" = { workspace = true }
"shared.e" = { workspace = true }
"grpc_client.a" = { workspace = true }
"grpc_client.b" = { workspace = true }
```

Since these dependencies don’t specify fixed versions, I suspect the `--frozen` option might be introducing some overhead. So I’m planning to tweak the setup a bit and run further tests.

---

_Comment by @timothy-jeong on 2025-06-14 04:25_

I specified versions for the locally created modules in the workspace, but the Docker container still consumes around 100MB more memory even when no requests are being processed.

However, when I remove the `--frozen` option, the memory usage drops.

---

_Comment by @timothy-jeong on 2025-06-25 02:18_

I solve this problem by
1.  Following uv [docker example reference](https://github.com/astral-sh/uv-docker-example), especially the multistage setup
2. Not including the `uv.lock` file in the Docker context, since I wanted to use the `--all-packages` option
3. Installing packages with uv sync `--all-packages --no-dev`

As a result, I was able to reduce both the final Docker image size and the container's memory usage.


this is my project structure and  full docker file for one specific serviec

```
.
├── packages
├── run_compose.sh
├── services
│   ├── a_service
│   │   ├── app
│   │   └── start_servers.sh
├── pyproject.toml
└── uv.lock
```


```docker
# Builder stage
FROM python:3.12-slim AS builder
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/uv

ENV UV_COMPILE_BYTECODE=1 UV_LINK_MODE=copy
ENV UV_PYTHON_DOWNLOADS=0

WORKDIR /app

# First, install root dependencies
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv sync --frozen --no-install-project --no-dev

# Add all source code
COPY pyproject.toml /app/pyproject.toml
COPY packages /app/packages
COPY services/a_service /app/services/a_service

# Install all packages at build time
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --all-packages --no-dev

# Runtime stage
FROM python:3.12-slim

# Create a non-root user
RUN groupadd -r app && useradd -r -g app app

# Copy the application from the builder
COPY --from=builder --chown=app:app /app /app

# Set virtual environment path
ENV PATH="/app/.venv/bin:$PATH"
ENV PYTHONPATH=/app:/app/services/a_service

USER app
WORKDIR /app/services/a_service

RUN chmod +x ./start_servers.sh
CMD ["/bin/bash", "./start_servers.sh"]

```

---

_Closed by @timothy-jeong on 2025-06-25 02:18_

---

_Comment by @charliermarsh on 2025-06-25 02:20_

Thanks for following up!

---
