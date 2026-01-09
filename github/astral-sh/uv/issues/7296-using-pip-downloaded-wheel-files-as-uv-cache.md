---
number: 7296
title: Using pip downloaded wheel files as uv cache 
type: issue
state: open
author: daler-rahimov
labels: []
assignees: []
created_at: 2024-09-11T15:54:31Z
updated_at: 2025-09-04T05:04:59Z
url: https://github.com/astral-sh/uv/issues/7296
synced_at: 2026-01-07T13:12:17-06:00
---

# Using pip downloaded wheel files as uv cache 

---

_Issue opened by @daler-rahimov on 2024-09-11 15:54_

I'm trying to use existing wheel files in `uv sync`, but I'm not able to. Here is a situation I have: I'm on a machine with a very slow internet connection, and I'm trying to limit data usage as much as possible. There are existing wheel files downloaded with `pip download --dest [path-to-pip-cache] ...` that I want `uv` to check before it goes to PyPi.

Here is the only way I found that almost accomplishes this:

```bash
pip3.9 download --dest [path-to-pip-cache] .
uv sync --cache-dir [uv-cache-path] --find-links [path-to-pip-cache] --no-index
```

If I don't include `--no-index`, `uv` is actually downloading everything from PyPi, and with `--no-index`, I have to make sure all the packages are downloaded to `[path-to-pip-cache]` with pip, which is slow. Also, `uv` has to change `uv.lock` and point to the local file system for all packages:

```bash
...
[[package]]
name = "alembic"
version = "1.4.3"
source = { registry = "../../opt/package-cache/pip" }
dependencies = [
    { name = "mako", marker = "python_full_version == '3.9.20'" },
    { name = "python-dateutil", marker = "python_full_version == '3.9.20'" },
    { name = "python-editor", marker = "python_full_version == '3.9.20'" },
    { name = "sqlalchemy", marker = "python_full_version == '3.9.20'" },
]
wheels = [
    { path = "alembic-1.4.3-py2.py3-none-any.whl" },
]
...
```

I tried to find out if `uv` caches downloaded packages somewhere so I can just copy mine there, but it looks like `uv` only stores installed/unzipped packages in its cache directory. I'm not sure if this is a missing feature or if there is some fundamental issue with storing and reusing downloaded wheel files.

---

_Comment by @lmmx on 2024-09-12 09:22_

Relates to #3163

---

_Referenced in [astral-sh/uv#3163](../../astral-sh/uv/issues/3163.md) on 2024-09-13 18:40_

---

_Comment by @vadimkantorov on 2025-09-04 04:49_

Supporting a variant of caching with only whl files is also important because it better allows storing the cache at a slow network drive (useful for sharing between lab machines). Wheels extracted to thousands of small files are not very friendly to network drives: keeping compressed whl files in cache might be a good option for this usecase (and simultaneously, this variant would become compatible with pip cache)

---
