```yaml
number: 9776
title: "`uv lock --no-build` fails when no build backend is specified in root package"
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2024-12-10T15:21:34Z
updated_at: 2024-12-10T20:10:52Z
url: https://github.com/astral-sh/uv/issues/9776
synced_at: 2026-01-10T04:36:21Z
```

# `uv lock --no-build` fails when no build backend is specified in root package

---

_Issue opened by @konstin on 2024-12-10 15:21_

`uv lock --no-build` fails when no build backend is specified. This is a false positive, since we never build the package when locking:

```
  × Failed to build `dummy @ file:///home/konsti/projects/dummy`
  ╰─▶ Building source distributions is disabled
```

**pyproject.toml**
```toml
[project]
name = "dummy"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []
```

The metadata in this package is static, we should skip the build, and indeed the lock messages (`-v`) don't show any build from happening. This can be verified by adding a bogus build backend to pyproject.toml, now the locking passes even with `--no-build`:

```toml
[build-system]
requires = ["bogus-does-not-exit"]
build-backend = "bogus-does-not-exit"
```

---

_Label `bug` added by @konstin on 2024-12-10 15:21_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-10 19:42_

---

_Closed by @charliermarsh on 2024-12-10 20:10_

---
