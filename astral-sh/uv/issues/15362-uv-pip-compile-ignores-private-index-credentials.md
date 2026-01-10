---
number: 15362
title: "`uv pip compile` ignores private index credentials if `pyproject.toml` is not in the current directory"
type: issue
state: closed
author: ketozhang
labels:
  - bug
assignees: []
created_at: 2025-08-18T21:33:16Z
updated_at: 2025-09-15T13:54:39Z
url: https://github.com/astral-sh/uv/issues/15362
synced_at: 2026-01-10T01:25:55Z
---

# `uv pip compile` ignores private index credentials if `pyproject.toml` is not in the current directory

---

_Issue opened by @ketozhang on 2025-08-18 21:33_

### Summary

My colleague (@sarmen-t) and I encountered that by following https://docs.astral.sh/uv/concepts/indexes/#providing-credentials-directly for injecting index credentials, `uv pip compile $SRC_FILE` fails to use credentials if $SRC_FILE is not exactly `pyproject.toml` (i.e., pyproject.toml cannot live in a subdirectory of CWD).

Here's a MWE (index url is faked)

```toml
[project]
name = "projectA"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ['internal-package']

[tool.uv.sources]
internal-package = { index = "internal-proxy" }

[[tool.uv.index]]
name = "internal-proxy"
url = "http://internal-proxy.example.com/simple/"
explicit = true
```

Contents of a multi-projects directory:
```console
$ ls
projectA  projectB  README.md
$ ls projectA
src  pyproject.toml
```

Fail case:

`--no-cache` is added to force auth & downloading from remote index.

```console
$ export UV_INDEX_INTERNAL_PROXY_USERNAME=public
$ export UV_INDEX_INTERNAL_PROXY_PASSWORD=koala
$ uv pip compile --format pylock.toml --no-cache projectA/pyproject.toml
error: Failed to fetch: `http://internal-proxy.example.com/packages/internal-package-3.3.1-py2.py3-none-any.whl`
  Caused by: HTTP status client error (401 Unauthorized) for url (http://internal-proxy.example.com/packages/internal-package-3.3.1-py2.py3-none-any.whl)
```

Successful alternatives:

```console
$ cd projectA
$ uv pip compile --format pylock.toml --no-cache pyproject.toml
```

```console
$ uv pip compile --format pylock.toml --no-cache --project projectA/ projectA/pyproject.toml
```


We can see from the last successful alternative that the issue is due to `--project` option which default to the CWD and is independent from `$SRC_FILE`. The error makes sense, but it is surprising that `uv pip compile` does not read the uv metadata table from the $SRC_FILE to obtain index metadata then subsequently its index credentials. 

### Platform

Darwin 24.6.0 x86_64

### Version

uv 0.8.11 (Homebrew 2025-08-14)

### Python version

Python 3.12.11

---

_Label `bug` added by @ketozhang on 2025-08-18 21:33_

---

_Renamed from "`uv pip compile` ignores private index credentials depending on current working directory" to "`uv pip compile` ignores private index credentials if `pyproject.toml` is not in the current directory" by @ketozhang on 2025-08-27 01:58_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-09-14 02:18_

---

_Referenced in [astral-sh/uv#15844](../../astral-sh/uv/pulls/15844.md) on 2025-09-15 00:11_

---

_Closed by @charliermarsh on 2025-09-15 13:54_

---
