---
number: 7919
title: "\"The discovered commit has an invalid length\" bug"
type: issue
state: closed
author: my1e5
labels:
  - bug
assignees: []
created_at: 2024-10-04T10:35:11Z
updated_at: 2024-10-04T11:10:55Z
url: https://github.com/astral-sh/uv/issues/7919
synced_at: 2026-01-07T13:12:17-06:00
---

# "The discovered commit has an invalid length" bug

---

_Issue opened by @my1e5 on 2024-10-04 10:35_

Set up a simple dynamic version library

```
$ uv init --lib foo
```
Edit the `pyproject.toml` to use a dynamic version, the hatch vcs plugin and uv `git=true` `cache-keys`.
```
[project]
name = "foo"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []
dynamic = ["version"]

[build-system]
requires = ["hatchling", "hatch-vcs"]
build-backend = "hatchling.build"

[tool.hatch.version]
source = "vcs"

[tool.uv]
cache-keys = [{ git = true }]
```
Run `uv sync`
```
$ uv sync
.
 + foo==0.1.dev0+d20241004 (from file:///C:/Users/.../foo)
.
```
All good so far - there isn't any Git commit history.

Make a commit and sync again.
```
$ git add .
$ git commit -m "initial"
$ uv -v sync
DEBUG uv 0.4.18
DEBUG Found project root: `C:\Users\me\...\foo`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `C:\Users\me\...\foo\.python-version`
DEBUG The virtual environment's Python version satisfies `Python 3.12`
DEBUG Using request timeout of 30s
DEBUG No static `pyproject.toml` available for: foo @ file:///C:/Users/me/.../foo (PyprojectToml(DynamicField("version")))
DEBUG Acquired lock for `C:\Users\me\AppData\Local\uv\cache\sdists-v4\editable\087dff0042ae766a`
DEBUG Failed to read the current commit: The discovered commit has an invalid length (expected 40 characters): `0f4599403b2f1736440acf2c215273e8d418e8ad
`
DEBUG Using cached metadata for: foo @ file:///C:/Users/me/.../foo
DEBUG No workspace root found, using project root
DEBUG Released lock at `C:\Users\me\AppData\Local\uv\cache\sdists-v4\editable\087dff0042ae766a\.lock`
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 1 package in 4ms
DEBUG Using request timeout of 30s
DEBUG Requirement installed, but not fresh: foo==0.1.dev0+d20241004 (from file:///C:/Users/me/.../foo)
DEBUG Failed to read the current commit: The discovered commit has an invalid length (expected 40 characters): `0f4599403b2f1736440acf2c215273e8d418e8ad
`
DEBUG Directory source requirement already cached: foo==0.1.dev0+d20241004 (from file:///C:/Users/me/.../foo)
DEBUG Uninstalled foo (7 files, 1 directory)
Uninstalled 1 package in 1ms
Installed 1 package in 11ms
 ~ foo==0.1.dev0+d20241004 (from file:///C:/Users/me/.../foo)
```

It seems uv is failing to read the commit correctly? 
```
DEBUG Failed to read the current commit: The discovered commit has an invalid length (expected 40 characters): 
```
It looks like it could be a newline problem? Is it including a newline character therefore taking it over 40 characters?

If you run `uv sync --reinstall` it works
```
$ uv sync --reinstall
.
Uninstalled 1 package in 2ms
Installed 1 package in 9ms
 - foo==0.1.dev0+d20241004 (from file:///C:/Users/me/.../foo)
 + foo==0.1.dev1+g0f45994 (from file:///C:/Users/me/.../foo)
 ```
 
Reproducible on macOS and Windows.
```
$ uv version
uv 0.4.18 (7b55e9790 2024-10-01)
```

---

_Referenced in [astral-sh/uv#7922](../../astral-sh/uv/pulls/7922.md) on 2024-10-04 11:04_

---

_Label `bug` added by @charliermarsh on 2024-10-04 11:04_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-04 11:04_

---

_Comment by @charliermarsh on 2024-10-04 11:04_

Thanks, fixed in https://github.com/astral-sh/uv/pull/7922.

---

_Closed by @charliermarsh on 2024-10-04 11:10_

---

_Closed by @charliermarsh on 2024-10-04 11:10_

---
