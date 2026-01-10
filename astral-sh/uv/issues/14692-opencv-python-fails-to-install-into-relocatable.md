---
number: 14692
title: opencv-python fails to install into relocatable non-.venv venv on Windows
type: issue
state: open
author: doctorpangloss
labels:
  - bug
assignees: []
created_at: 2025-07-17T20:48:09Z
updated_at: 2025-07-17T20:53:50Z
url: https://github.com/astral-sh/uv/issues/14692
synced_at: 2026-01-10T01:25:47Z
---

# opencv-python fails to install into relocatable non-.venv venv on Windows

---

_Issue opened by @doctorpangloss on 2025-07-17 20:48_

### Summary

opencv-python and similar isn't installing at all in a venv set up with `uv venv --relocatable --python 3.12 python_embedded`
```
(python_embedded) PS D:\appmana\appmana-comfyui\src\python_embedded_test> uv pip install opencv-python
Using Python 3.12.10 environment at: python_embedded
Audited 1 package in 7ms
(python_embedded) PS D:\appmana\appmana-comfyui\src\python_embedded_test> uv pip install opencv-python-headless
Using Python 3.12.10 environment at: python_embedded
Audited 1 package in 7ms
(python_embedded) PS D:\appmana\appmana-comfyui\src\python_embedded_test> uv pip install opencv-python-contrib
Using Python 3.12.10 environment at: python_embedded
Audited 1 package in 7ms
```

other stuff installs just fine?

```
(python_embedded) PS D:\appmana\appmana-comfyui\src\python_embedded_test> uv pip install wandb
Using Python 3.12.10 environment at: python_embedded
Resolved 21 packages in 208ms
Prepared 3 packages in 493ms
Installed 3 packages in 219ms
 + platformdirs==4.3.8
 + sentry-sdk==2.33.0
 + wandb==0.21.0
```

uv run misbehaves too:

```
(python_embedded) PS D:\appmana\appmana-comfyui\src\python_embedded_test> uv run python --version --active
warning: `VIRTUAL_ENV=python_embedded` does not match the project environment path `D:\appmana\.venv` and will be ignored; use `--active` to target the active environment instead
⠙ Resolving dependencies...
(python_embedded) PS D:\appmana\appmana-comfyui\src\python_embedded_test> ls


    Directory: D:\appmana\appmana-comfyui\src\python_embedded_test


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         7/17/2025   1:13 PM                python_embedded

```

### Platform

Windows 2022

### Version

uv 0.7.22 (78d6d1134 2025-07-17)

### Python version

Python 3.12

---

_Label `bug` added by @doctorpangloss on 2025-07-17 20:48_

---

_Comment by @doctorpangloss on 2025-07-17 20:53_

seems to be related to having a "project" in a higher level directory

---
