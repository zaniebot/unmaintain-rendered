---
number: 11558
title: pytorch cannot be installed from cpu index when opencv is also a dependency
type: issue
state: closed
author: Dronakurl
labels:
  - bug
assignees: []
created_at: 2025-02-16T16:39:56Z
updated_at: 2025-02-16T17:05:40Z
url: https://github.com/astral-sh/uv/issues/11558
synced_at: 2026-01-07T13:12:18-06:00
---

# pytorch cannot be installed from cpu index when opencv is also a dependency

---

_Issue opened by @Dronakurl on 2025-02-16 16:39_

### Summary

When following the [instructions on using uv with pytorch on the  astral website](https://docs.astral.sh/uv/guides/integration/pytorch/#using-a-pytorch-index), I encounter an error when adding opencv.

I get the error
```
3.807 error: Distribution `torch==2.6.0 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform
3.807
3.807 hint: You're on Linux (`manylinux_2_36_x86_64`), but `torch` (v2.6.0) only has wheels for the following platform: `macosx_11_0_arm64`
```

Here is the Dockerfile with minimal example that produces this error. Without opencv-python, it works, so torch is installed.
I read #11406, but could not find anything that helps me with that.

```Dockerfile
FROM ghcr.io/astral-sh/uv:0.5.24-debian-slim

COPY <<EOF /mre/pyproject.toml
[project]
name = "project"
version = "0.1.0"
requires-python = "==3.12.0"
dependencies = [
  "torch>1.13.1'",
  "torchvision>=0.14.1",
  "opencv-python",
]

[tool.uv.sources]
torch = [{ index = "pytorch-cpu" }]
torchvision = [{ index = "pytorch-cpu" }]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
EOF

WORKDIR /mre
RUN uv  sync -U 
```

Run with 
`docker build -t uvdebug . --progress plain --no-cache`

### Platform

Ubuntu 24.04, x86_64

### Version

uv 0.6.0

### Python version

Python 3.12.3

---

_Label `bug` added by @Dronakurl on 2025-02-16 16:39_

---

_Comment by @charliermarsh on 2025-02-16 17:02_

I fixed this on main in #11546.

---

_Closed by @charliermarsh on 2025-02-16 17:02_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-16 17:02_

---

_Comment by @Dronakurl on 2025-02-16 17:05_

Thank you very much! I missed the other issue in my search.

---

_Comment by @charliermarsh on 2025-02-16 17:05_

No worries, it's not at all obvious that it's the same issue, I just learned when debugging that it was _also_ triggered by OpenCV in the end.

---
