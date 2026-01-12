```yaml
number: 14921
title: "Missing uv build backend option in docs in `uv init --help` and online docs"
type: issue
state: closed
author: abidsikder
labels:
  - bug
assignees: []
created_at: 2025-07-27T20:45:48Z
updated_at: 2025-07-28T13:26:24Z
url: https://github.com/astral-sh/uv/issues/14921
synced_at: 2026-01-12T16:01:59Z
```

# Missing uv build backend option in docs in `uv init --help` and online docs

---

_@abidsikder_

### Summary

The option to specify a uv build backend is missing in `uv init --help` `--build-backend`'s docs. The option still works though if you pass in `uv`, not sure if there are others that also work. I only found this through accident.

The option is also missing in the online documentation.

<img width="964" height="455" alt="Image" src="https://github.com/user-attachments/assets/4c39c8be-9012-4950-95f5-1c02e432a7a0" />

# To Reproduce
`uv init --help`
You'll see 
```
--build-backend <BUILD_BACKEND>  Initialize a build-backend of choice for the project [env: UV_INIT_BUILD_BACKEND=] [possible values: hatch,
                                       flit, pdm, poetry, setuptools, maturin, scikit]
```
but run
```
uv init --build-backend uv test-project
tail -n 3 test-project/pyproject.toml
```
and you'll see 
```
[build-system]
requires = ["uv_build>=0.8.3,<0.9.0"]
build-backend = "uv_build"
```
so the option must just be missing from the documentation.

### Platform

macos 15 arm64

### Version

uv 0.8.3 (7e78f54e7 2025-07-24)

### Python version

NA

---

_Label `bug` added by @abidsikder on 2025-07-27 20:45_

---

_Comment by @zanieb on 2025-07-28 13:14_

Thanks!

---

_Closed by @zanieb on 2025-07-28 13:26_

---
