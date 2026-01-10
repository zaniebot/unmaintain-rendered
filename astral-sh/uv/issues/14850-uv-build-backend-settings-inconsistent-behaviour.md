---
number: 14850
title: "uv build backend settings: inconsistent behaviour among Linux architectures"
type: issue
state: closed
author: benoit9126
labels:
  - question
assignees: []
created_at: 2025-07-23T16:20:34Z
updated_at: 2025-07-23T17:22:03Z
url: https://github.com/astral-sh/uv/issues/14850
synced_at: 2026-01-10T01:25:49Z
---

# uv build backend settings: inconsistent behaviour among Linux architectures

---

_Issue opened by @benoit9126 on 2025-07-23 16:20_

### Summary

Hi!

I am using the `uv` build backend with a pure Python package. I can use the `uv sync` command to install it on my computer (Debian 12). Then I would like to use `uv sync` in a `musllinux` environment, and it fails because I have a sequence of `module-name` (not a single string).

The error message is
```
error: Failed to parse: `pyproject.toml`
  Caused by: TOML parse error at line 14, column 15
   |
14 | module-name = ["cloud.database", "cloud.database_pro"]
   |               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
invalid type: sequence, expected a string
```

Here is a Dockerfile to reproduce the problem

```Dockerfile
FROM quay.io/pypa/musllinux_1_2_x86_64:2025.06.28-1
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

WORKDIR /app/

# Create a project
RUN uv venv --python /opt/python/cp311-cp311/bin/python \
    && mkdir -p src/cloud/database \
    && mkdir -p src/cloud/database_pro \
    && touch src/cloud/database/__init__.py \
    && touch src/cloud/database_pro/__init__.py

# Create a Pyproject using uv build backend and a sequence module-name
RUN echo -e '[project]\n\
name = "app"\n\
version = "0.1.0"\n\
description = "Add your description here"\n\
readme = "README.md"\n\
requires-python = ">=3.11"\n\
dependencies = []\n\
\n\
[build-system]\n\
requires = ["uv_build>=0.8.2,<0.9.0"]\n\
build-backend = "uv_build"\n\
\n\
[tool.uv.build-backend]\n\
module-name = ["cloud.database", "cloud.database_pro"]' > /app/pyproject.toml

# This command fails
RUN uv sync

ENTRYPOINT [ "sh" ]
```

If you replace the first line of this Dockerfile with `FROM quay.io/pypa/manylinux_2_34_x86_64:latest`, `uv` accepts a sequence as `module-name` and the `uv sync` command works fine.

I would have expected a consistent behaviour among different architectures.


### Platform

Alpine Linux v3.22

### Version

uv 0.7.15

### Python version

Python 3.11.13

---

_Label `bug` added by @benoit9126 on 2025-07-23 16:20_

---

_Renamed from "uv build backend: inconsistent behaviour among linux architecture" to "uv build backend settings: inconsistent behaviour among Linux architectures" by @benoit9126 on 2025-07-23 16:22_

---

_Comment by @zanieb on 2025-07-23 16:36_

Sounds like you're on different uv versions, we added multiple module name support in a subsequent version.

---

_Label `bug` removed by @zanieb on 2025-07-23 16:36_

---

_Label `question` added by @zanieb on 2025-07-23 16:36_

---

_Comment by @zanieb on 2025-07-23 16:36_

Does it reproduce if you pin the uv version you're copying from?

---

_Comment by @benoit9126 on 2025-07-23 17:22_

It works fine when I force the last version of `uv` (0.8.2). I will force the update of `uv` in my cibuildwheel process in order to always have the last version of `uv` among different architectures.

Thanks a lot!

---

_Closed by @benoit9126 on 2025-07-23 17:22_

---
