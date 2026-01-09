---
number: 16609
title: Issue Installing uv on AIX via pip
type: issue
state: closed
author: shenxianpeng
labels:
  - question
assignees: []
created_at: 2025-11-06T10:29:05Z
updated_at: 2025-11-08T03:08:55Z
url: https://github.com/astral-sh/uv/issues/16609
synced_at: 2026-01-07T13:12:19-06:00
---

# Issue Installing uv on AIX via pip

---

_Issue opened by @shenxianpeng on 2025-11-06 10:29_

### Question

Thanks for creating such an awesome tool!

I'm trying to install uv on an AIX system using pip, but the installation fails with the following error. Does uv support installation on AIX?

```
 pip install uv
Collecting uv
  Using cached uv-0.9.7.tar.gz (3.7 MB)
  Installing build dependencies ... error
  error: subprocess-exited-with-error

  installing build dependencies for uv did not run successfully.
  exit code: 1

  [30 lines of output]
  Collecting maturin<2.0,>=1.0
    Using cached maturin-1.9.6.tar.gz (214 kB)
    Installing build dependencies: started
    Installing build dependencies: finished with status 'done'
    Getting requirements to build wheel: started
    Getting requirements to build wheel: finished with status 'done'
    Installing backend dependencies: started
    Installing backend dependencies: finished with status 'done'
    Preparing metadata (pyproject.toml): started
    Preparing metadata (pyproject.toml): finished with status 'error'
    error: subprocess-exited-with-error

    Preparing metadata (pyproject.toml) did not run successfully.
    exit code: 1

    [3 lines of output]
    Python reports SOABI: cpython-312
    Unsupported platform: 312
    Rust not found, installing into a temporary directory
    [end of output]

    note: This error originates from a subprocess, and is likely not a problem with pip.
  error: metadata-generation-failed

  Encountered error while generating package metadata.

  maturin

  note: This is an issue with the package mentioned above, not pip.
  hint: See above for details.
  [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
ERROR: Failed to build 'uv' when installing build dependencies for uv
```

```
(venv) bash-5.2# python3 -V
Python 3.12.12
(venv) bash-5.2# pip --version
pip 25.3 from /workspace/venv/lib64/python3.12/site-packages/pip (python 3.12)
```

Also tried the `install.sh` it shows does not support AIX.

```
curl -LsSf https://astral.sh/uv/install.sh | sh
ERROR: unrecognized OS type: AIX
```

### Platform

AIX (7300-02-01-2346)

### Version

0.9.7

---

_Label `question` added by @shenxianpeng on 2025-11-06 10:29_

---

_Comment by @konstin on 2025-11-06 16:16_

Does the installation work if you're installing Rust manually? rustup doesn't support AIX, and uv requires Rust, which makes the installation currently fails.

---

_Comment by @shenxianpeng on 2025-11-06 17:11_

> Does the installation work if you're installing Rust manually?

Thanks for the reply. I don't know if AIX supports Rust but will try to install Rust then.

---

_Closed by @charliermarsh on 2025-11-08 03:08_

---
