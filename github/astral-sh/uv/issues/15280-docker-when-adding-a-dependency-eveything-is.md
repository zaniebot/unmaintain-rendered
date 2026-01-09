---
number: 15280
title: Docker - When adding a dependency eveything is installed
type: issue
state: closed
author: errajibadr
labels:
  - question
assignees: []
created_at: 2025-08-14T13:33:15Z
updated_at: 2025-08-14T13:59:40Z
url: https://github.com/astral-sh/uv/issues/15280
synced_at: 2026-01-07T13:12:19-06:00
---

# Docker - When adding a dependency eveything is installed

---

_Issue opened by @errajibadr on 2025-08-14 13:33_

### Question

Description
When I add a new dependency to my project using ⁠uv add package, the Docker build step that runs ⁠uv sync --no-install-project reinstalls all dependencies instead of just the new one.
I've structured my Dockerfile in two stages to prevent source code changes from triggering a full dependency reinstall, but this doesn't seem to work when adding new dependencies.

Expected behavior

When adding a new dependency with ⁠uv add package, only the new package should be installed during the Docker build, leveraging the cache for existing dependencies.

Actual behavior
All dependencies in docker Build are reinstalled from scratch when ⁠pyproject.toml or ⁠uv.lock changes.
Questions
1.	Is there a way to avoid full dependency reinstallation when adding a new package? 	
2.	Am I structuring my Dockerfile incorrectly for this use case? 	
3.	Are there any best practices for optimizing Docker builds with ⁠uv when dependencies change frequently?


```
FROM python:3.12.8-slim-bookworm

ENV UV_COMPILE_BYTECODE=1
ENV PYTHONUNBUFFERED=1
ENV UV_LINK_MODE=copy
ENV UV_PROJECT_ENVIRONMENT=/usr/local 

WORKDIR /app

# Install the project's dependencies using the lockfile and settings
RUN --mount=from=ghcr.io/astral-sh/uv,source=/uv,target=/bin/uv \
    --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv sync --locked --no-install-project --no-dev


COPY pyproject.toml uv.lock ./
COPY src/ ./src
RUN --mount=from=ghcr.io/astral-sh/uv,source=/uv,target=/bin/uv \
    uv sync --no-dev

COPY scripts/ ./scripts
COPY migrations/ ./migrations

# ENV PYTHONPATH=/app/src
# ENV PATH="/app/.venv/bin:$PATH"
RUN chmod +x ./scripts/start.sh

EXPOSE 8080

CMD ["./scripts/start.sh"]

```

### Platform

MacOS - Darwin

### Version

_No response_

---

_Label `question` added by @errajibadr on 2025-08-14 13:33_

---

_Comment by @zanieb on 2025-08-14 13:38_

> When adding a new dependency with ⁠uv add package, only the new package should be installed during the Docker build, leveraging the cache for existing dependencies.

The environment is not cached, so we need to install all of the packages still. We will leverage the cache, e.g., avoiding downloads? but the install still needs to happen.

---

_Comment by @errajibadr on 2025-08-14 13:49_

Fair enough.
Thanks for fast reply. 
However speed of install is not the same as local system. 

Is this a known issue/normal behavior ? 

## Docker
[stage-0 3/9] RUN --mount=from=ghcr.io/astral-sh/uv,source=/uv,target=/bin/uv     --mount=type=cache,target=/root/.cache/uv     --mount=type=bind,source=uv.lock,target=uv.lock     --mount=type=bind,source=pyproject.toml,target=pyproject.toml     uv sync --locked --no-install-project --no-dev
#9 0.263 Resolved 177 packages in 6ms
#9 0.500 Downloading zstandard (4.7MiB)
#9 0.899  Downloading zstandard
#9 0.954 Prepared 9 packages in 644ms
#9 5.998 Installed 144 packages in 5.04s
#9 8.249 Bytecode compiled 8186 files in 2.25s

## Local
(genai-template) ➜  genai-launchpad git:(working_realtime_chat) ✗ uv sync --reinstall --no-dev                                        
Resolved 177 packages in 0.89ms
   Built dataunboxed-launchpad @ file:///Users/badrou/repository/genai-launchpad
Prepared 144 packages in 787ms
Uninstalled 173 packages in 1.63s
Installed 144 packages in 369ms

---

_Comment by @zanieb on 2025-08-14 13:56_

Usually the cache in Docker is on a different file system so we need to copy the files instead of hard linking like we do on your local machine

---

_Closed by @errajibadr on 2025-08-14 13:59_

---
