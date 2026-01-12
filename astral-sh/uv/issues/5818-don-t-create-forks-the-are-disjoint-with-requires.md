```yaml
number: 5818
title: "Don't create forks the are disjoint with requires-python"
type: issue
state: closed
author: konstin
labels:
  - performance
  - resolver
  - preview
assignees: []
created_at: 2024-08-06T16:25:31Z
updated_at: 2024-08-16T16:32:41Z
url: https://github.com/astral-sh/uv/issues/5818
synced_at: 2026-01-12T15:58:59Z
```

# Don't create forks the are disjoint with requires-python

---

_@konstin_

Usually, we only consider the lower bound of requires-python in the resolver. For when forking, it doesn't make sense to consider forks above the upper bound, e.g. with `==3.11.*`, a fork `python_version >= '3.13'` doesn't make sense.

`pyproject.toml`:

```toml
[project]
name = "warehouse"
version = "1.0.0"
requires-python = "==3.11.*"
dependencies = [
  "pydantic",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

Head of `uv.lock`:

```toml
version = 1
requires-python = ">=3.11, <3.12"
environment-markers = [
    "python_version < '3.13'",
    "python_version >= '3.13'",
]
```

---

_Label `resolver` added by @konstin on 2024-08-06 16:25_

---

_Label `preview` added by @konstin on 2024-08-06 16:25_

---

_Label `performance` added by @konstin on 2024-08-09 13:28_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-09 14:19_

---

_Comment by @charliermarsh on 2024-08-15 18:45_

We now do this for Python versions that are below the lower bound, at least.

---

_Comment by @charliermarsh on 2024-08-16 13:35_

Going to close this and create a separate issue around sometimes respecting upper-bound.

---

_Closed by @charliermarsh on 2024-08-16 16:32_

---
