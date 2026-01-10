```yaml
number: 16301
title: "Feature request: Add option to ignore missing/unavailable indexes"
type: issue
state: open
author: rabyj
labels:
  - enhancement
assignees: []
created_at: 2025-10-14T19:01:50Z
updated_at: 2025-12-15T18:00:25Z
url: https://github.com/astral-sh/uv/issues/16301
synced_at: 2026-01-10T03:11:35Z
```

# Feature request: Add option to ignore missing/unavailable indexes

---

_Issue opened by @rabyj on 2025-10-14 19:01_

### Summary


Currently, `uv pip install` fails when an index defined in `pyproject.toml` is unavailable, producing errors like:

```
error: Failed to read `--find-links` directory: /cvmfs/soft.computecanada.ca/custom/python/wheelhouse/generic
  Caused by: No such file or directory (os error 2)
```

**Requested feature:**
Add an option to treat missing indexes as warnings rather than errors, allowing uv to continue dependency resolution using the remaining available indexes.

**Use case:**
I want to configure a local wheelhouse as the primary index using `index-strategy = "unsafe-first-match"` in `pyproject.toml`. When the local wheelhouse is available, uv should search it first. When it's unavailable (e.g., on developer machines without the mounted path), uv should gracefully skip it and fall back to subsequent indexes (like PyPI) instead of failing.

This would enable portable configurations that work across different environments without requiring environment-specific `pyproject.toml` files.

### Example

**Example usage:**

**Current behavior (fails):**
```toml
# pyproject.toml
[tool.uv]
index-strategy = "unsafe-first-match"

[[tool.uv.index]]
url = "/cvmfs/soft.computecanada.ca/custom/python/wheelhouse/generic"
name = "local-wheelhouse"

[[tool.uv.index]]
url = "https://pypi.org/simple"
name = "pypi"
```

```bash
$ uv pip install numpy
error: Failed to read `--find-links` directory: /cvmfs/soft.computecanada.ca/custom/python/wheelhouse/generic
  Caused by: No such file or directory (os error 2)
# Installation fails completely
```

**Desired behavior (with new option):**
```toml
# pyproject.toml - same config with new option
[tool.uv]
index-strategy = "unsafe-first-match"
ignore-missing-indexes = true  # or similar flag name
```

```bash
$ uv pip install numpy
warning: Index 'local-wheelhouse' is unavailable: /cvmfs/soft.computecanada.ca/custom/python/wheelhouse/generic (os error 2)
Resolved 1 package in 245ms
Installed 1 package in 12ms
 + numpy==1.26.4
# Installation succeeds using PyPI fallback
```

This allows the same `pyproject.toml` to work seamlessly on both cluster environments (where CVMFS is mounted) and local development machines (where it isn't).

---

_Label `enhancement` added by @rabyj on 2025-10-14 19:01_

---

_Renamed from "Add option to ignore missing/unavailable indexes" to "Feature request: Add option to ignore missing/unavailable indexes" by @rabyj on 2025-12-15 18:00_

---
