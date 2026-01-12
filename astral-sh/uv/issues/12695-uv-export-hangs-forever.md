```yaml
number: 12695
title: "`uv export` hangs forever"
type: issue
state: closed
author: fabito
labels:
  - bug
assignees: []
created_at: 2025-04-07T02:06:13Z
updated_at: 2025-04-07T19:11:00Z
url: https://github.com/astral-sh/uv/issues/12695
synced_at: 2026-01-12T16:01:10Z
```

# `uv export` hangs forever

---

_@fabito_

### Summary

uv export hangs forever, unless ray dependency is upgraded

Here is the dockerfile to reproduce the error:

```Dockerfile
FROM --platform=linux/amd64 ghcr.io/astral-sh/uv:python3.11-bookworm-slim

COPY <<EOF /mre/pyproject.toml
[project]
name = "uv-export-stuck"
version = "0.0.1"
requires-python = "~=3.11"
dependencies = [
    "ray[train, default, data]==2.32.0",   # Hangs forever
    # "ray[train, default, data]",         # Works fine !
]

[project.optional-dependencies]
cpu = [
  "torch>=2.2",
  "torchvision>=0.18.1",
]
gpu = [
  "torch>=2.2",
  "torchvision>=0.18.1",
]

[tool.uv]
package = false
conflicts = [
  [
    { extra = "cpu" },
    { extra = "gpu" },
  ],
]

[[tool.uv.index]]
name = "pypi"
url = "https://pypi.org/simple"

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pypi", extra = "gpu" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pypi", extra = "gpu" },
]

EOF

WORKDIR /mre
RUN uv lock && uv export --extra cpu --no-dev --verbose

```


### Platform

Darwin 22.6.0 arm64

### Version

uv 0.6.12 (e4e03833f 2025-04-02)

### Python version

Python 3.11.10

---

_Label `bug` added by @fabito on 2025-04-07 02:06_

---

_Renamed from "bug: uv export hangs forever" to "bug: `uv export` hangs forever" by @fabito on 2025-04-07 02:06_

---

_Renamed from "bug: `uv export` hangs forever" to "`uv export` hangs forever" by @fabito on 2025-04-07 10:41_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-07 18:02_

---

_Closed by @charliermarsh on 2025-04-07 19:11_

---

_Closed by @charliermarsh on 2025-04-07 19:11_

---
