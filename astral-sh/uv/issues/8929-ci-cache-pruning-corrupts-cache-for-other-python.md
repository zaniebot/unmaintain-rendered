```yaml
number: 8929
title: CI cache pruning corrupts cache for other Python versions
type: issue
state: closed
author: nijel
labels:
  - bug
  - cache
assignees: []
created_at: 2024-11-08T10:24:20Z
updated_at: 2024-11-08T20:12:16Z
url: https://github.com/astral-sh/uv/issues/8929
synced_at: 2026-01-12T15:59:38Z
```

# CI cache pruning corrupts cache for other Python versions

---

_@nijel_

We use multiple Python versions in one of our CI pipelines, but the uv cache is generated only for one of the Python versions. On subsequent runs when cache is used, the package build fails with:

```
Resolved 1 package in 107ms
error: Failed to prepare distributions
  Caused by: Failed to download and build `pycairo==1.27.0`
  Caused by: Attempted to re-extract the source distribution for `pycairo==1.27.0`, but the hashes didn't match. Run `uv cache clean` to clear the cache.
```

Clearing the cache indeed fixes the issue. 

The issue does not exist without running `uv cache prune --ci`.

Reproduced with uv 0.4.29 and 0.5.0.

Reproducer:

```sh
#!/bin/sh

export UV_CACHE_DIR="$PWD/cache"

rm -rf "$UV_CACHE_DIR" .venv .venv-2 pyproject.toml uv.lock

uv init --name uv-cache-issue
uv add --python 3.13 "pycairo"

uv cache prune --ci

rm -rf .venv .venv-2

uv venv --python python3.11 .venv-2
. .venv-2/bin/activate
uv pip install "pycairo"
```

---

_Label `bug` added by @charliermarsh on 2024-11-08 13:47_

---

_Label `cache` added by @charliermarsh on 2024-11-08 13:47_

---

_Comment by @charliermarsh on 2024-11-08 13:47_

Sounds like a bug though not obvious why this would happen.

---

_Comment by @charliermarsh on 2024-11-08 15:50_

I think the proximate problem is that `uv add` generates hashes by default but `uv pip install` does not.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-08 15:50_

---

_Closed by @charliermarsh on 2024-11-08 20:12_

---

_Closed by @charliermarsh on 2024-11-08 20:12_

---
