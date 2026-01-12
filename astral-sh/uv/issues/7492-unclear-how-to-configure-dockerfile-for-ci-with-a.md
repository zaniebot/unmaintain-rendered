```yaml
number: 7492
title: Unclear how to configure dockerfile for CI with a workspace and multiple projects
type: issue
state: closed
author: vilim
labels: []
assignees: []
created_at: 2024-09-18T11:41:15Z
updated_at: 2024-10-25T19:09:55Z
url: https://github.com/astral-sh/uv/issues/7492
synced_at: 2026-01-12T15:59:14Z
```

# Unclear how to configure dockerfile for CI with a workspace and multiple projects

---

_@vilim_

I have a workspace setup as follows
```
pyproject.toml
uv.lock
package1
   pyproject.toml
package2
   pyproject.toml
```
with the workspace .toml containing
```toml
[tool.uv.workspace]
members = ["package1", "package2"]

[tool.uv.sources]
package1 = { workspace = true }
package2 = { workspace = true }
```
and package2 depends on package1, so package2 toml contains
```toml
[project]
name = "package2"
...
dependencies = [
    "package1",
    ...
    ]
```

Then in the dockerfile
```dockerfile
ARG BASE_IMAGE="ghcr.io/astral-sh/uv:python3.12-bookworm-slim"

FROM ${BASE_IMAGE} as baseimage

WORKDIR /app
COPY pyproject.toml uv.lock /app/
COPY project1/pyproject.toml /app/project1/pyproject.toml
COPY project2/pyproject.toml /app/project2/pyproject.toml

# Enable bytecode compilation
ENV UV_COMPILE_BYTECODE=1

ENV UV_LINK_MODE=copy
## Install the project's dependencies using the lockfile and settings
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv sync --frozen --no-install-workspace
    
ADD project1 project2 /app/

RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --frozen

WORKDIR /app/project2
```

And when trying to run the tests, even running `uv tree` as part of the CI action in this image I get the error
```
Using Python 3.12.6 interpreter at: /usr/local/bin/python3
error: Because package1 was not found in the package registry and your project depends on package1, we can conclude that your project's requirements are unsatisfiable.
```

Everything works without issues locally when running e.g. `uv run pytest`  within `project1` or `project2`

---

_Comment by @vilim on 2024-10-25 19:09_

duplicate of #8460 , fixed!

---

_Closed by @vilim on 2024-10-25 19:09_

---
