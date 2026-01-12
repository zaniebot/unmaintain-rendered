```yaml
number: 9969
title: new build backend - sdist not installable with pip?
type: issue
state: closed
author: davidszotten
labels:
  - bug
  - preview
assignees: []
created_at: 2024-12-17T12:43:57Z
updated_at: 2024-12-18T19:14:10Z
url: https://github.com/astral-sh/uv/issues/9969
synced_at: 2026-01-12T16:00:03Z
```

# new build backend - sdist not installable with pip?

---

_@davidszotten_

Hi,

I may be doing something wrong here but i was expecting this to work. (build sdist with uv backend, then install it with pip)

```
$ cat pyproject.toml
[project]
name = "test-uv-build-backend"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

[build-system]
requires = ["uv>=0.5,<0.6"]
build-backend = "uv"
```

```
$ uv --version
uv 0.5.9 (0652800cb 2024-12-13)
```

```
$ UV_PREVIEW=1 uv --preview build
Building source distribution (uv build backend)...
Building wheel from source distribution (uv build backend)...
Successfully built dist/test_uv_build_backend-0.1.0.tar.gz
Successfully built dist/test_uv_build_backend-0.1.0-py3-none-any.whl
```

```
$ mktmpenv
[...]
$ pip --version
pip 24.3.1 from /Users/david/environments/tmp-b0ff1617aea3df9/lib/python3.12/site-packages/pip (python 3.12)

$ UV_PREVIEW=1 pip install ~/dev/test-uv-build-backend/dist/test_uv_build_backend-0.1.0.tar.gz
Processing /Users/david/dev/test-uv-build-backend/dist/test_uv_build_backend-0.1.0.tar.gz
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: test-uv-build-backend
  Building wheel for test-uv-build-backend (pyproject.toml) ... error
  error: subprocess-exited-with-error

  × Building wheel for test-uv-build-backend (pyproject.toml) did not run successfully.
  │ exit code: 2
  ╰─> [1 lines of output]
      error: failed to open file `/private/var/folders/3q/26_y3lv165q8vhq94rjp80qw0000gp/T/pip-modern-metadata-gpvtfr9u/test_uv_build_backend-0.1.0.dist-info/test_uv_build_backend-0.1.0.dist-info/METADATA`: No such file or directory (os error 2)
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
  ERROR: Failed building wheel for test-uv-build-backend
Failed to build test-uv-build-backend
ERROR: ERROR: Failed to build installable wheels for some pyproject.toml based projects (test-uv-build-backend)
```



---

_Renamed from "new build backend - source not installable with pip?" to "new build backend - sdist not installable with pip?" by @davidszotten on 2024-12-17 12:44_

---

_Label `bug` added by @zanieb on 2024-12-17 15:38_

---

_Label `preview` added by @zanieb on 2024-12-17 15:38_

---

_Assigned to @konstin by @zanieb on 2024-12-17 15:38_

---

_Closed by @konstin on 2024-12-18 19:14_

---
