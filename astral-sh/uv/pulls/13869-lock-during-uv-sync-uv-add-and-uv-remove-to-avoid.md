```yaml
number: 13869
title: "Lock during `uv sync`, `uv add` and `uv remove` to avoid race conditions"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/lock-uv-sync
created_at: 2025-06-05T17:48:08Z
updated_at: 2025-06-06T12:16:42Z
url: https://github.com/astral-sh/uv/pull/13869
synced_at: 2026-01-12T16:10:54Z
```

# Lock during `uv sync`, `uv add` and `uv remove` to avoid race conditions

---

_@konstin_

Surprisingly, we weren't locking during `uv sync` so far, so running `uv sync` in parallel could cause errors in filesystem races.

I've also added locks to `uv add` and `uv remove` which concurrently modify `pyproject.toml`. These locks only apply after we determined the interpreter, so they don't actually prevent computing the same thing twice when running `uv add` in parallel.

All other subcommands that I checked were already locking (with no claim to exhaustiveness)

Fixes #12751

# Test Plan

I don't have CI-sized reproducer for this.

```toml
[project]
name = "debug"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
  "boto3>=1.38.30",
  "fastapi>=0.115.12",
  "numba>=0.61.2",
  "polars>=1.30.0",
  "protobuf>=6.31.1",
  "pyarrow>=20.0.0",
  "pydantic>=2.11.5",
  "requests>=2.32.3",
  "urllib3>=2.4.0",
  "scikit-learn>=1.6.1",
  "jupyter>=1.1.1",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

```
rm -rf .venv && parallel -n0 "uv sync -q" ::: {1..10}
```


---

_Label `bug` added by @konstin on 2025-06-05 17:48_

---

_Comment by @leviska on 2025-06-05 19:22_

Does this also lock on `uv run` if it needs sync?

---

_@Gankra approved on 2025-06-06 04:11_

`uv version` also presumably needs this

---

_Comment by @konstin on 2025-06-06 12:16_

Tracking follow-ups in #13883

---

_Merged by @konstin on 2025-06-06 12:16_

---

_Closed by @konstin on 2025-06-06 12:16_

---

_Branch deleted on 2025-06-06 12:16_

---
