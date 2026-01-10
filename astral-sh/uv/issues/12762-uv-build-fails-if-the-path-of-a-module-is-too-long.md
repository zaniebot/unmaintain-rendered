---
number: 12762
title: uv-build fails if the path of a module is too long
type: issue
state: closed
author: leiserfg
labels:
  - bug
assignees: []
created_at: 2025-04-08T20:50:27Z
updated_at: 2025-04-09T13:01:23Z
url: https://github.com/astral-sh/uv/issues/12762
synced_at: 2026-01-10T01:25:24Z
---

# uv-build fails if the path of a module is too long

---

_Issue opened by @leiserfg on 2025-04-08 20:50_

### Summary

When using 

```toml
[build-system]
requires = ["uv_build>=0.6,<0.7"]
build-backend = "uv_build"
```
if the full path of a file that will go in the `sdist` is long, uv_build will fail like:

```sh
Caused by:
    provided value is too long when setting path for <a path>
```
This looks like the same error that the `tar` crate shows when trying to write a `tar.gz` in `ustar` mode. But I checked the code of uv_build and only see `gnu` mode which should not have that kind of problem. So either this is already fixed in master (in that case I don't know how to test it) or is it is still broken and I'm checking in the wrong place. 

 To reproduce the issue:
use this pyproject:
```toml
[project]
name = "asdf"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = []

[build-system]
requires = ["uv_build>=0.6,<0.7"]
build-backend = "uv_build"
```
and this commands  to generate the folders:
```
mkdir -p src/asdf/asdf11111111111111111111111111111111111111111111111111111111111111111111111111111111111111/
touch src/asdf/__init__.py
touch src/asdf/asdf11111111111111111111111111111111111111111111111111111111111111111111111111111111111111/__init__.py

uv build
```


### Platform

nixos-unstable

### Version

0.6.12

### Python version

Python 3.13

Edit: update to version 0.6.12

---

_Label `bug` added by @leiserfg on 2025-04-08 20:50_

---

_Assigned to @konstin by @charliermarsh on 2025-04-08 20:55_

---

_Comment by @charliermarsh on 2025-04-08 20:55_

\cc @konstin 

---

_Comment by @leiserfg on 2025-04-08 21:19_

I think I found it, 
here https://github.com/astral-sh/uv/blob/e0b4dfe923ee807ed2e6e9e16e101a652977ef42/crates/uv-build-backend/src/source_dist.rs#L323-L324

`header.set_path` does not handles long paths regardless the `gnu` mode. 

---

_Referenced in [astral-sh/uv#12764](../../astral-sh/uv/pulls/12764.md) on 2025-04-08 21:31_

---

_Closed by @konstin on 2025-04-09 13:01_

---

_Closed by @konstin on 2025-04-09 13:01_

---
