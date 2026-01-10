---
number: 15086
title: "Running `uv build` in Docker container: `symlink destination for /usr/bin/python3 is outside of the target directory`"
type: issue
state: closed
author: alechouse97
labels:
  - bug
assignees: []
created_at: 2025-08-05T17:45:26Z
updated_at: 2025-08-05T17:48:17Z
url: https://github.com/astral-sh/uv/issues/15086
synced_at: 2026-01-10T01:25:52Z
---

# Running `uv build` in Docker container: `symlink destination for /usr/bin/python3 is outside of the target directory`

---

_Issue opened by @alechouse97 on 2025-08-05 17:45_

### Summary

I am using uv to build a whl file in a docker container as part of a Gitlab CI/CD pipeline. However, since uv 0.8.1, we get the following error when running `uv build`:

```
$ python --version
Python 3.13.5
$ which python
/usr/local/bin/python
$ uv --version
uv 0.8.4
$ uv build
Building source distribution...
  × Failed to build `/builds/myproject`
  ├─▶ I/O operation failed during extraction
  ├─▶ failed to unpack
  │   `/builds/myproject/.uv-cache/sdists-v9/.tmpPnC5xs/myproject-1.0.1/.uv-cache/builds-v0/.tmpTVvP5v/bin/python`
  ╰─▶ symlink destination for /usr/bin/python3 is outside of the target directory
Cleaning up project directory and file based variables 00:01
ERROR: Job failed: exit code 1
```

Relevant areas of `pyproject.toml`:
```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

And the `.gitlab-ci.yaml`:
```yaml
variables:
  PYTHON_VERSION: "3.13"
  IMAGE: ghcr.io/astral-sh/uv:python$PYTHON_VERSION-bookworm
  UV_CACHE_DIR: .uv-cache
  UV_SYSTEM_PYTHON: 1
  UV_LINK_MODE: copy
```

I am able to build fine within a virtual environment on a local system, but this fails using the system python install in the base uv image.


### Platform

Debian Bookworm x86_64

### Version

0.8.4

### Python version

3.13.5

---

_Label `bug` added by @alechouse97 on 2025-08-05 17:45_

---

_Closed by @alechouse97 on 2025-08-05 17:48_

---

_Referenced in [astral-sh/uv#15096](../../astral-sh/uv/issues/15096.md) on 2025-08-06 00:16_

---
