---
number: 12309
title: Install library in editable mode in dev, but from a private repo in prod
type: issue
state: open
author: andrej-peterka
labels:
  - question
assignees: []
created_at: 2025-03-19T08:07:58Z
updated_at: 2025-10-02T12:36:16Z
url: https://github.com/astral-sh/uv/issues/12309
synced_at: 2026-01-10T01:25:18Z
---

# Install library in editable mode in dev, but from a private repo in prod

---

_Issue opened by @andrej-peterka on 2025-03-19 08:07_

### Question

Hi,

I'm developing a couple of scripts (`tool_01`, `tool_02`) that depend on a common library (`lib_common`).
Everything is developed by me and I have a private repo to where I publish the library.

Final step is building my `tool_01` as a docker image and publishing it to a docker registry.

This is my folder structure:
```
lib_common
|-- README.md
|-- pyproject.toml
|-- src
|   `-- lib_common
|       |-- __init__.py
|       |-- py.typed
|       `-- my_code
|           |-- __init__.py
`-- uv.lock

tool_01
|-- Dockerfile
|-- README.md
|-- pyproject.toml
|-- src
|   |-- docker-entrypoint.py
|   `-- tool.py
`-- uv.lock
```

The `lib_common` and `tool_01` folders are in the same folder, one next to the other.

`tool_01` includes `lib_common` as a dependency:

```
[project]
name = "tool-01"
version = "0.1.0"
classifiers = [
    "Private :: Do Not Upload"
]
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "requests~=2.32.3",
    "lib-common==0.1.0",
]

[tool.uv.sources]
lib-common = [
    { index = "private-repo" }
]

[[tool.uv.index]]
name = "private-repo"
url = "https://.../simple/"
explicit = true
```

And this works, no issues.

The question I have is how can I develop `lib-common` and have the changes be reflected in the `tool_01` project without building, publishing and re-instaling the `lib-common`?

I know that installing `lib-common` in editable mode is the solution, but how can I incorporate that into the `pyproject.toml` and the lock file?

I tried adding this to the `tool_01` pyproject.toml:

```
[dependency-groups]
dev = [
    "lib-common",
]

[tool.uv.sources]
lib-common = [
    { path = "../lib_common", editable = true, group = "dev" },
    { index = "private-repo" }
]
```

But then my docker build of the `tool_01` fails with error:
```
0.481 Using CPython 3.13.2 interpreter at: /usr/local/bin/python3
0.482 Creating virtual environment at: .venv
0.525 error: Failed to determine installation plan
0.525   Caused by: Distribution not found at: file:///lib_common
```

which makes sense, since that folder is not present in my docker build - I want it to use the package from my private repo there.

Dockerfile:
```
ARG BUILD_PLATFORM=linux/amd64
FROM --platform=${BUILD_PLATFORM} ghcr.io/astral-sh/uv:python3.13-bookworm-slim AS builder

ENV UV_COMPILE_BYTECODE=1 UV_LINK_MODE=copy UV_PYTHON_DOWNLOADS=0

WORKDIR /src

RUN --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv sync --frozen --no-install-project --no-dev

ADD src/ /src/

FROM --platform=${BUILD_PLATFORM} python:3.13-slim AS final

# Copy the application from the builder
COPY --from=builder /src /src

# Place executables in the environment at the front of the path
ENV PATH="/src/.venv/bin:$PATH"
```

I saw some related issues, but after reading through them, I'm not sure what the solution to my question is...

https://github.com/astral-sh/uv/issues/9654
https://github.com/astral-sh/uv/issues/11104
https://github.com/astral-sh/uv/issues/11363
https://github.com/astral-sh/uv/issues/11675

### Platform

macOS, docker, linux

### Version

uv 0.6.7

---

_Label `question` added by @andrej-peterka on 2025-03-19 08:07_

---

_Comment by @edenian-prince on 2025-10-01 17:55_

Did you ever figure out a solution to this? I'm running into a similar issue where one project needs a package to be pulled from a certain repo, and another project needs to pull that package from a different repo. Like this

```bash
[tool.uv.sources]
lib-common = [
    { index = "github/repo/package", group = "default" },
    { index = "private-repo", group = "dev" }
]
```

---

_Comment by @andrej-peterka on 2025-10-02 12:36_

No, I haven't found a solution. I might revisit this soon-ish.

---
