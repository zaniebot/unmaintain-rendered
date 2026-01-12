```yaml
number: 17390
title: "UV_NO_SYNC=1 doesn't work as expected"
type: issue
state: closed
author: ravenkls
labels:
  - bug
assignees: []
created_at: 2026-01-09T20:43:46Z
updated_at: 2026-01-09T21:37:22Z
url: https://github.com/astral-sh/uv/issues/17390
synced_at: 2026-01-12T16:02:50Z
```

# UV_NO_SYNC=1 doesn't work as expected

---

_@ravenkls_

### Summary

In the latest version 0.9.23 when I use `UV_NO_SYNC=1 uv run ruff format` it doesn't work as expected (it is install dependencies).

My pyproject.toml contains
```toml

[project]
...
dependencies = [
  "deepdiff>=5.8.0",
  "httpx>=0.24.1,<0.29",
]

[dependency-groups]
dev = [
  "pytest>=8.0.0",
  "pre-commit>=3.7.0",
  "ruff>=0.14.1",
  "coverage[toml]>=7.10.3",
  "django>=5.2.8",
]
```

When I run 
```sh
uv sync --only-group dev
UV_NO_SYNC=1 uv run ruff format
```

The `uv run ruff format` commands syncs.

### Platform

macOS 26 Darwin 25.0.0 arm64

### Version

0.9.23

### Python version

Python 3.12.8

---

_Label `bug` added by @ravenkls on 2026-01-09 20:43_

---

_Comment by @zanieb on 2026-01-09 20:50_

Thanks for the report.

I assume this does not reproduce in 0.9.22?

---

_Comment by @ravenkls on 2026-01-09 20:55_

No I can't reproduce in 0.9.22

---

_Closed by @zanieb on 2026-01-09 21:37_

---
