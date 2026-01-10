---
number: 6062
title: Extras are sometimes not collapsed properly
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
created_at: 2024-08-13T17:06:04Z
updated_at: 2024-08-14T13:55:41Z
url: https://github.com/astral-sh/uv/issues/6062
synced_at: 2026-01-10T01:23:55Z
---

# Extras are sometimes not collapsed properly

---

_Issue opened by @charliermarsh on 2024-08-13 17:06_

Given:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "datasets < 2.19 ; sys_platform == 'darwin' ",
    "datasets >= 2.19 ; sys_platform == 'win32'"
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

```

Produces:

```toml
[[package]]
name = "datasets"
version = "2.18.0"
source = { registry = "https://pypi.org/simple" }
environment-markers = [
    "sys_platform == 'darwin'",
]
dependencies = [
    { name = "aiohttp", marker = "sys_platform == 'darwin'" },
    { name = "dill", marker = "sys_platform == 'darwin'" },
    { name = "filelock", marker = "sys_platform == 'darwin'" },
    { name = "fsspec", marker = "sys_platform == 'darwin'" },
    { name = "fsspec", extra = ["http"] },
    { name = "huggingface-hub", marker = "sys_platform == 'darwin'" },
    { name = "multiprocess", marker = "sys_platform == 'darwin'" },
    { name = "numpy", marker = "sys_platform == 'darwin'" },
    { name = "packaging", marker = "sys_platform == 'darwin'" },
    { name = "pandas", marker = "sys_platform == 'darwin'" },
    { name = "pyarrow", marker = "sys_platform == 'darwin'" },
    { name = "pyarrow-hotfix", marker = "sys_platform == 'darwin'" },
    { name = "pyyaml", marker = "sys_platform == 'darwin'" },
    { name = "requests", marker = "sys_platform == 'darwin'" },
    { name = "tqdm", marker = "sys_platform == 'darwin'" },
    { name = "xxhash", marker = "sys_platform == 'darwin'" },
]
sdist = { url = "https://files.pythonhosted.org/packages/ff/d5/d0fffd6afdf24c062e3c289f0b13f7636f074005c1e76e322633e8c5508c/datasets-2.18.0.tar.gz", hash = "sha256:cdf8b8c6abf7316377ba4f49f9589a4c74556d6b481afd0abd2284f3d69185cb", size = 589972 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/95/fc/661a7f06e8b7d48fcbd3f55423b7ff1ac3ce59526f146fda87a1e1788ee4/datasets-2.18.0-py3-none-any.whl", hash = "sha256:f1bbf0e2896917a914de01cbd37075b14deea3837af87ad0d9f697388ccaeb50", size = 510519 },
]
```

Notice `{ name = "fsspec", extra = ["http"] }` alongside `{ name = "fsspec", marker = "sys_platform == 'darwin'" }`.

---

_Label `bug` added by @charliermarsh on 2024-08-13 17:06_

---

_Label `preview` added by @charliermarsh on 2024-08-13 17:06_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-13 17:06_

---

_Comment by @charliermarsh on 2024-08-13 17:38_

That actually doesn't _quite_ repro as-is, I had a change on my branch. Let me find a clearer repro (now that I see the underlying problem).

---

_Comment by @charliermarsh on 2024-08-13 17:51_

Edited to include a real repro.

---

_Referenced in [astral-sh/uv#6065](../../astral-sh/uv/pulls/6065.md) on 2024-08-13 18:01_

---

_Closed by @charliermarsh on 2024-08-14 13:55_

---

_Closed by @charliermarsh on 2024-08-14 13:55_

---
