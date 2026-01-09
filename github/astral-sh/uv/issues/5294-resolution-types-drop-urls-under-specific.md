---
number: 5294
title: "`Resolution` types drop urls under specific circumstances"
type: issue
state: closed
author: konstin
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-22T17:17:32Z
updated_at: 2024-07-22T22:41:04Z
url: https://github.com/astral-sh/uv/issues/5294
synced_at: 2026-01-07T13:12:17-06:00
---

# `Resolution` types drop urls under specific circumstances

---

_Issue opened by @konstin on 2024-07-22 17:17_

## Summary

The problem: The fields below (and the subsequent petgraph code) are missing the url, so we can't tell apart edges that are only different by their url, but equal by package name and version. This means in specific cases like the one in the section below we drop a marker and install the wrong package.

https://github.com/astral-sh/uv/blob/5a23f0579962da26dbf117ca008822c26f5d15e5/crates/uv-resolver/src/resolver/mod.rs#L2375-L2380

## Reproduction

Create two dependency edges in two different forks that have the same starting package name and version and the same target package name and version, but are different in their url.

```toml
[project]
name = "a3"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
  "a @ file:///home/ferris/projects/uv/debug/a1 ; python_version >='3.11'",
  "a @ file:///home/ferris/projects/uv/debug/a2 ; python_version <'3.11'"
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.metadata]
allow-direct-references = true
```

`/home/ferris/projects/uv/debug/a1`:
```toml
[project]
name = "a"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
dependencies = [
    "anyio==4.3.0",
]

[tool.uv]
dev-dependencies = []

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

`/home/ferris/projects/uv/debug/a2`:
```toml
[project]
name = "a"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
dependencies = [
    "anyio==4.4.0",
]

[tool.uv]
dev-dependencies = []

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

In the lockfile, we're indeed missing the marker:

```toml
[[distribution]]
name = "a"
version = "0.1.0"
source = { directory = "/home/konsti/projects/uv/debug/a1" }
dependencies = [
    { name = "anyio", version = "4.3.0", source = { registry = "https://pypi.org/simple" } },
    { name = "anyio", version = "4.4.0", source = { registry = "https://pypi.org/simple" } },
]
```

Instead of respecting the markers, we always install `a3` and `anyio==4.4.0`  for python 3.10-3.12.

---

_Label `bug` added by @konstin on 2024-07-22 17:17_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-22 17:26_

---

_Comment by @charliermarsh on 2024-07-22 17:26_

Thanks, makes sense.

---

_Label `preview` added by @konstin on 2024-07-22 17:29_

---

_Referenced in [astral-sh/uv#5301](../../astral-sh/uv/pulls/5301.md) on 2024-07-22 18:03_

---

_Referenced in [astral-sh/uv#5312](../../astral-sh/uv/pulls/5312.md) on 2024-07-22 21:12_

---

_Closed by @charliermarsh on 2024-07-22 22:41_

---

_Closed by @charliermarsh on 2024-07-22 22:41_

---

_Referenced in [astral-sh/uv#5253](../../astral-sh/uv/issues/5253.md) on 2024-07-29 00:54_

---
