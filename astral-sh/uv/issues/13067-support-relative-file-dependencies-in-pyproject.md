---
number: 13067
title: "Support relative file:// dependencies in pyproject.toml for uv sync"
type: issue
state: closed
author: drsshz
labels:
  - question
assignees: []
created_at: 2025-04-23T12:04:45Z
updated_at: 2025-04-24T16:18:01Z
url: https://github.com/astral-sh/uv/issues/13067
synced_at: 2026-01-10T01:25:28Z
---

# Support relative file:// dependencies in pyproject.toml for uv sync

---

_Issue opened by @drsshz on 2025-04-23 12:04_


**Description**:

### Problem Statement

When using `uv sync`, relative `file://` dependencies declared in `pyproject.toml` are not supported. For example:

```toml
[project.dependencies]
shared-utils = { url = "file://../shared-utils" }
```

This results in an error:

```
relative path without a working directory: ../shared-utils
```

---

### Why This Is a Problem

- `uv pip install -e ../shared-utils` works, but bypasses `pyproject.toml`, breaking reproducibility and automation.
- Absolute paths like `file:///Users/me/packages/shared-utils` are not portable across machines or CI.
- Tools like `pip` and `pip-tools` already support this, making this gap more noticeable.

---

### What This Feature Would Enable

Support in `uv sync` for relative local paths using PEP 440 direct references:

```toml
shared-utils @ file://../shared-utils
```

This would allow:

- Portable local dependencies
- Monorepo-friendly development
- Clean and complete dependency declaration inside `pyproject.toml`
- Avoidance of external install commands, scripts, or Makefiles

---


---

_Comment by @konstin on 2025-04-23 12:48_

You can use `[tool.uv.sources]` for relative path dependencies: https://docs.astral.sh/uv/concepts/projects/dependencies/#path. Relative paths in URLs are a non-standard feature that uv doesn't support, instead we provide `[tool.uv.sources]`.

---

_Label `question` added by @charliermarsh on 2025-04-24 16:18_

---

_Closed by @charliermarsh on 2025-04-24 16:18_

---
