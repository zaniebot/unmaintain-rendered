---
number: 17023
title: "`uv run` always syncs dev dependencies in docker container"
type: issue
state: closed
author: jcayzac
labels:
  - bug
assignees: []
created_at: 2025-12-08T09:10:17Z
updated_at: 2025-12-08T09:21:23Z
url: https://github.com/astral-sh/uv/issues/17023
synced_at: 2026-01-10T01:26:12Z
---

# `uv run` always syncs dev dependencies in docker container

---

_Issue opened by @jcayzac on 2025-12-08 09:10_

### Summary

I am seeing this strange behavior where `uv run` insists on installing my `dev` dependencies despites all the flags I've tried below:

```dockerfile
FROM python:3.14.1-alpine3.23
WORKDIR /app
COPY --from=ghcr.io/astral-sh/uv:0.9.16-python3.14-trixie /uv /usr/local/bin/uv
COPY pyproject.toml .
COPY uv.lock .
COPY .python-version .
COPY src/ ./src/

# ✅ Does not install dev dependencies during build
RUN uv sync --frozen --no-default-groups --no-dev --exact

# ❌ Always installs dev dependencies when the container starts
ENTRYPOINT [ \
  "uv", \
  "run", \
  "--locked", \
  "--frozen", \
  "--no-default-groups", \
  "--no-sync", \
  "--group", \
  "", \
  "--no-group", \
  "dev", \
  "run-agent" \
]
```

In my `pyproject.toml`, my default dependencies are declared under

```toml
[project]
dependencies = [ ... ]
```

…and my dev dependencies under

```toml
[dependency-groups]
dev = [ ... ]
```

I cannot reproduce the problem on my host machine (same version of `uv` and `python`, but macos).

### Platform

Alpine 3.23

### Version

0.9.16-python3.14-trixie

### Python version

3.14.1

---

_Label `bug` added by @jcayzac on 2025-12-08 09:10_

---

_Comment by @jcayzac on 2025-12-08 09:21_

Oops, the image wasn't being rebuilt, never mind!

---

_Closed by @jcayzac on 2025-12-08 09:21_

---
