```yaml
number: 17026
title: Django uv docker setup
type: issue
state: closed
author: blossomsg
labels:
  - question
assignees: []
created_at: 2025-12-08T09:54:17Z
updated_at: 2025-12-09T15:35:13Z
url: https://github.com/astral-sh/uv/issues/17026
synced_at: 2026-01-12T16:02:42Z
```

# Django uv docker setup

---

_@blossomsg_

### Question

Hello All,
I am exploring uv with docker, but facing some challenges setting up **Dockerfile**

for the sake of upload i have added an extension .txt
[Dockerfile.txt](https://github.com/user-attachments/files/24029033/Dockerfile.txt)

I am referring django for professionals book
in which they are setting pipenv with docker, but i wanted to explore with uv
I keep getting the below error


```
 => ERROR [stage-0 8/8] RUN --mount=type=cache,target=/root/.cache/uv     uv sync --locked --no-dev                                                                                                                                                                         0.2s 
------
 > [stage-0 8/8] RUN --mount=type=cache,target=/root/.cache/uv     uv sync --locked --no-dev:
0.220 error: failed to create directory `/nonexistent/.cache/uv`: Permission denied (os error 13)
------
Dockerfile:61
--------------------
  60 |     COPY . /app
  61 | >>> RUN --mount=type=cache,target=/root/.cache/uv \
  62 | >>>     uv sync --locked --no-dev
  63 |
--------------------
ERROR: failed to build: failed to solve: process "/bin/sh -c uv sync --locked --no-dev" did not complete successfully: exit code: 2

```
here is the docker file snippet from the book.
<img width="587" height="907" alt="Image" src="https://github.com/user-attachments/assets/97e2d7a5-072b-434a-a95b-2cc94f708512" />

I referred these links
https://docs.astral.sh/uv/guides/integration/docker/#using-uv-in-docker
https://github.com/astral-sh/uv-docker-example/blob/main/Dockerfile

Did the necessary tweaks, but still facing permissions issue.
Do let me know what needs to be refactored, it would be really helpful
Thank You.

### Platform

Microsoft Windows 11 Pro x64

### Version

uv 0.8.2 (21fadbcc1 2025-07-22)

---

_Label `question` added by @blossomsg on 2025-12-08 09:54_

---

_Renamed from "Djanog uv docker setup" to "Django uv docker setup" by @AlexWaygood on 2025-12-08 09:55_

---

_Comment by @zanieb on 2025-12-08 13:08_

Copying this inline for readability
```Dockerfile
# syntax=docker/dockerfile:1

# Comments are provided throughout this file to help you get started.
# If you need more help, visit the Dockerfile reference guide at
# https://docs.docker.com/go/dockerfile-reference/

# Want to help us make this template better? Share your feedback here: https://forms.gle/ybq9Krt8jtBL3iCk7

FROM python:3.12-slim-bookworm
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/


# Setup a non-root user
RUN groupadd --system --gid 999 nonroot \
 && useradd --system --gid 999 --uid 999 --create-home nonroot

# Prevents Python from writing pyc files.
ENV PYTHONDONTWRITEBYTECODE=1

# Keeps Python from buffering stdout and stderr to avoid situations where
# the application crashes without emitting any logs due to buffering.
ENV PYTHONUNBUFFERED=1

WORKDIR /app

# Enable bytecode compilation
ENV UV_COMPILE_BYTECODE=1

# Copy from the cache instead of linking since it's a mounted volume
ENV UV_LINK_MODE=copy

# Ensure installed tools can be executed out of the box
ENV UV_TOOL_BIN_DIR=/usr/local/bin

# Create a non-privileged user that the app will run under.
# See https://docs.docker.com/go/dockerfile-user-best-practices/
ARG UID=10001
RUN adduser \
    --disabled-password \
    --gecos "" \
    --home "/nonexistent" \
    --shell "/sbin/nologin" \
    --no-create-home \
    --uid "${UID}" \
    appuser

# Download dependencies as a separate step to take advantage of Docker's caching.
# Leverage a cache mount to /root/.cache/pip to speed up subsequent builds.
# Leverage a bind mount to requirements.txt to avoid having to copy them into
# into this layer.
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv sync --locked --no-install-project --no-dev

# Switch to the non-privileged user to run the application.
USER appuser

# Copy the source code into the container.
COPY . /app
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --locked --no-dev

# Place executables in the environment at the front of the path
ENV PATH="/app/.venv/bin:$PATH"

# Reset the entrypoint, don't invoke `uv`
ENTRYPOINT []

# Use the non-root user to run our application
USER nonroot


# Expose the port that the application listens on.
EXPOSE 8000

# Run the application.
CMD ["uv", "run", "python", "manage.py", "runserver", "0.0.0.0:8000"]
```

---

_Comment by @zanieb on 2025-12-08 13:09_

You change the home directory to `--home "/nonexistent"` so uv attempts to store the cache there. You have some mounts for the uv cache `--mount=type=cache,target=/root/.cache/uv` but note that uses `/root` instead of `/nonexistent`

---

_Comment by @blossomsg on 2025-12-08 14:38_

Hello @zanieb ,
thanks for the revert.

I tried both ways
 `--home "/nonexistent"`
```
    --gecos "" \
    --home "/nonexistent" \
    --shell "/sbin/nologin" \
```
```
RUN --mount=type=cache,target=/nonexistent/.cache/uv \
    uv sync --locked --no-dev
```
and same with `--home "/root"`
```
    --gecos "" \
    --home "/root" \
    --shell "/sbin/nologin" \
```
```
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --locked --no-dev
```
But unfortunately i am still getting same error, my last attempt was with `"/root"`, that is why the path of cache is with root.
```
 => ERROR [stage-0 8/8] RUN --mount=type=cache,target=/root/.cache/uv     uv sync --locked --no-dev                                                                                                                                                                         0.2s 
------
 > [stage-0 8/8] RUN --mount=type=cache,target=/root/.cache/uv     uv sync --locked --no-dev:
0.178 error: failed to create directory `/root/.cache/uv`: Permission denied (os error 13)
------
Dockerfile:60
--------------------
  59 |     COPY . /app
  60 | >>> RUN --mount=type=cache,target=/root/.cache/uv \
  61 | >>>     uv sync --locked --no-dev
  62 |
--------------------
ERROR: failed to build: failed to solve: process "/bin/sh -c uv sync --locked --no-dev" did not complete successfully: exit code: 2

```



---

_Comment by @zanieb on 2025-12-08 15:12_

You're switching to the unprivileged user early

```

# Switch to the non-privileged user to run the application.
USER appuser

# Copy the source code into the container.
COPY . /app
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --locked --no-dev
```

Do that later?

---

_Comment by @blossomsg on 2025-12-09 15:34_

Amazing @zanieb , thanks alot for the help.
Just worked in one go😁.
attaching the new formatted dockerfile. 

```dockerfile
# syntax=docker/dockerfile:1

# Comments are provided throughout this file to help you get started.
# If you need more help, visit the Dockerfile reference guide at
# https://docs.docker.com/go/dockerfile-reference/

# Want to help us make this template better? Share your feedback here: https://forms.gle/ybq9Krt8jtBL3iCk7

FROM python:3.12-slim-bookworm
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/


# Setup a non-root user
RUN groupadd --system --gid 999 nonroot \
 && useradd --system --gid 999 --uid 999 --create-home nonroot

# Prevents Python from writing pyc files.
ENV PYTHONDONTWRITEBYTECODE=1

# Keeps Python from buffering stdout and stderr to avoid situations where
# the application crashes without emitting any logs due to buffering.
ENV PYTHONUNBUFFERED=1

WORKDIR /app

# Enable bytecode compilation
ENV UV_COMPILE_BYTECODE=1

# Copy from the cache instead of linking since it's a mounted volume
ENV UV_LINK_MODE=copy

# Ensure installed tools can be executed out of the box
ENV UV_TOOL_BIN_DIR=/usr/local/bin

# Create a non-privileged user that the app will run under.
# See https://docs.docker.com/go/dockerfile-user-best-practices/
ARG UID=10001
RUN adduser \
    --disabled-password \
    --gecos "" \
    --home "/root" \
    --shell "/sbin/nologin" \
    --no-create-home \
    --uid "${UID}" \
    appuser

# Download dependencies as a separate step to take advantage of Docker's caching.
# Leverage a cache mount to /root/.cache/pip to speed up subsequent builds.
# Leverage a bind mount to requirements.txt to avoid having to copy them into
# into this layer.
RUN --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv sync --locked --no-install-project --no-dev


# Copy the source code into the container.
COPY . /app
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --locked --no-dev

# Switch to the non-privileged user to run the application.
USER appuser

# Place executables in the environment at the front of the path
ENV PATH="/app/.venv/bin:$PATH"

# Reset the entrypoint, don't invoke `uv`
ENTRYPOINT []

# Use the non-root user to run our application
USER nonroot

# Expose the port that the application listens on.
EXPOSE 8000

# Run the application.
CMD ["uv", "run", "python", "manage.py", "runserver", "0.0.0.0:8000"]
```


---

_Closed by @blossomsg on 2025-12-09 15:35_

---
