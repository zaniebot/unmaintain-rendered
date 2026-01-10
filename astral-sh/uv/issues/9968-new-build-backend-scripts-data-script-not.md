---
number: 9968
title: "new build backend - scripts data: script not executable in wheel"
type: issue
state: closed
author: davidszotten
labels:
  - bug
assignees: []
created_at: 2024-12-17T11:30:57Z
updated_at: 2024-12-19T16:54:46Z
url: https://github.com/astral-sh/uv/issues/9968
synced_at: 2026-01-10T01:24:48Z
---

# new build backend - scripts data: script not executable in wheel

---

_Issue opened by @davidszotten on 2024-12-17 11:30_

Hi,

excited to try out the new build backend and happened to be looking at scripts data:

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

[tool.uv.build-backend.data]
scripts = "scripts"
```

```
$ cat scripts/my-bin
#!/usr/bin/env bash

echo foo
```

```
$ ls -al scripts/my-bin
-rwxr-xr-x  1 david  staff  30 17 Dec 11:00 scripts/my-bin
   ^
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
$ zipinfo dist/test_uv_build_backend-0.1.0-py3-none-any.whl
Archive:  dist/test_uv_build_backend-0.1.0-py3-none-any.whl
Zip file size: 2050 bytes, number of entries: 9
drwxr-xr-x  4.6 unx        0 b- stor 80-Jan-01 00:00 test_uv_build_backend/
-rw-r--r--  4.6 unx       99 b- defN 80-Jan-01 00:00 test_uv_build_backend/__init__.py
drwxr-xr-x  4.6 unx        0 b- stor 80-Jan-01 00:00 test_uv_build_backend-0.1.0.data/scripts/
drwxr-xr-x  4.6 unx        0 b- stor 80-Jan-01 00:00 test_uv_build_backend-0.1.0.data/scripts/
-rw-r--r--  4.6 unx       30 b- defN 80-Jan-01 00:00 test_uv_build_backend-0.1.0.data/scripts/my-bin
drwxr-xr-x  4.6 unx        0 b- stor 80-Jan-01 00:00 test_uv_build_backend-0.1.0.dist-info/
-rw-r--r--  4.6 unx       78 b- defN 80-Jan-01 00:00 test_uv_build_backend-0.1.0.dist-info/WHEEL
-rw-r--r--  4.6 unx      165 b- defN 80-Jan-01 00:00 test_uv_build_backend-0.1.0.dist-info/METADATA
-rw-r--r--  4.6 unx      521 b- defN 80-Jan-01 00:00 test_uv_build_backend-0.1.0.dist-info/RECORD
9 files, 893 bytes uncompressed, 634 bytes compressed:  29.0%
```

note script isn't executable

```
-rw-r--r--  4.6 unx       30 b- defN 80-Jan-01 00:00 test_uv_build_backend-0.1.0.data/scripts/my-bin
   ^
```

---

_Assigned to @konstin by @konstin on 2024-12-17 11:31_

---

_Label `bug` added by @konstin on 2024-12-17 11:31_

---

_Referenced in [astral-sh/uv#10027](../../astral-sh/uv/pulls/10027.md) on 2024-12-19 12:05_

---

_Closed by @konstin on 2024-12-19 16:54_

---

_Closed by @konstin on 2024-12-19 16:54_

---
