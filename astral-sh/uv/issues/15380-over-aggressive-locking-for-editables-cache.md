---
number: 15380
title: Over-aggressive locking for editables cache
type: issue
state: open
author: SquidDev
labels:
  - bug
  - performance
assignees: []
created_at: 2025-08-19T13:15:22Z
updated_at: 2025-08-21T09:43:07Z
url: https://github.com/astral-sh/uv/issues/15380
synced_at: 2026-01-10T01:25:55Z
---

# Over-aggressive locking for editables cache

---

_Issue opened by @SquidDev on 2025-08-19 13:15_

### Summary

In an attempt to improve CI speeds, I've recently switched my workplaces self-hosted CI runners to use a persistent uv cache. This cache is *shared* across runners, to avoid wasting disk space — as [noted in the documentation](https://docs.astral.sh/uv/concepts/cache/#cache-safety), this should (and does!) work fine.

However, I've noticed some inefficiencies when building editable installs with native code (e.g. with maturin). `uv` acquires a lock on `$UV_CACHE_DIR/sdists-v9/editable/$CACHE_KEY` before starting a build. This means if two CI runners attempt to build the same package, one runner must wait for the other to finish building, before it builds itself.

This probably makes sense for pure-Python projects (where the build is cheap and reusable). However, because these packages use native code, we can't restore from the cache (see https://github.com/astral-sh/uv/issues/11390), so this feels doubly wasteful!

I wonder if it would be possible to reduce the scope of the lock, so that it's only acquired when the build finishes (and we write back the result to the cache), rather than the complete duration of the build?

## Reproduction steps
Create a project with the following layout ([premade one here](https://github.com/user-attachments/files/21856281/uv_build.zip))

```
├── build.py
├── pyproject.toml
├── src
│   └── uv_demo
│       └── __init__.py
```

Where `pyproject.toml` contains

```toml
[project]
name = "uv-demo"
version = "0.1.0"
requires-python = ">=3.8"
dependencies = []

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.wheel.hooks.custom]
path = "build.py"
```

and `build.py` just ensures the build takes some time:

```python
import time
from hatchling.builders.hooks.plugin.interface import BuildHookInterface

class CustomBuildHook(BuildHookInterface):
    def initialize(self, version: str, build_data: dict) -> None:
        time.sleep(10)
```

Then, we can kick off two builds at the same time with `docker run --rm -it --mount type=bind,src=$PWD,dst=/src -w /src --env RUST_LOG=uv_build_frontend=debug,uv_fs=debug,maturin=debug --env UV_CACHE_DIR=/src/cache --tmpfs /src/.venv ghcr.io/astral-sh/uv:debian uv sync --reinstall`. Watching the logs, we can see we spend ~10 seconds waiting for the editable lock for the second build:

```
DEBUG Acquired lock for `/src`
DEBUG Acquired lock for `/root/.local/share/uv/python`
Downloading cpython-3.8.20-linux-x86_64-gnu (download) (19.9MiB)
 Downloading cpython-3.8.20-linux-x86_64-gnu (download)
DEBUG Released lock at `/root/.local/share/uv/python/.lock`
Using CPython 3.8.20
Creating virtual environment at: .venv
DEBUG Released lock at `/tmp/uv-a307630218d6a300.lock`
DEBUG Acquired lock for `.venv`
Resolved 1 package in 0.96ms
INFO Waiting to acquire lock for `/src/cache/sdists-v9/editable/1de208056d3879b5` at `cache/sdists-v9/editable/1de208056d3879b5/.lock`
# ^^^^^^ Pauses here for 10s
DEBUG Acquired lock for `/src/cache/sdists-v9/editable/1de208056d3879b5`
DEBUG Released lock at `/src/cache/sdists-v9/editable/1de208056d3879b5/.lock`
Prepared 1 package in 8.46s
warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
Installed 1 package in 0.92ms
 + uv-demo==0.1.0 (from file:///src)
DEBUG Released lock at `/src/.venv/.lock`
```



### Platform

Linux 6.6.87.2-microsoft-standard-WSL2 x86_64 GNU/Linux, Ubuntu 24.04.3

### Version

uv 0.8.12

### Python version

Python 3.12.3

---

_Label `bug` added by @SquidDev on 2025-08-19 13:15_

---

_Label `performance` added by @zanieb on 2025-08-19 13:47_

---

_Comment by @charliermarsh on 2025-08-20 14:01_

I think part of the problem here is that it depends on the build backend. For example, IIRC `setuptools` is not safe to run concurrently.

---

_Comment by @SquidDev on 2025-08-21 09:43_

Oh, to clarify, the reproduction steps in the original issue are not especially representative! In practice, we're just using normal GHA self-hosted runners. So each build gets its own checkout of the repository, it's just the checkout is mounted in its build container at the same path, and so shares a cache key. So I don't *think* the issues with setuptools ([assuming you're referring to this?](https://github.com/astral-sh/uv/blob/8d6ea3f2ea3255acdd91f7b618014aef95f7b393/crates/uv-build-frontend/src/lib.rs#L481-L485), which huh TIL — that's nasty!) matter here, as the source tree isn't shared.

That all said, I realise this setup is a bit niche, and honestly mostly exists to work around other limitations with caching (#11390 and #11455/#15224). I'd originally opened this in the hopes it might be a simple fix, but looking through `uv_distribution::source`, I'm less sure that's the case — happy if you want to close as wontfix!

---
