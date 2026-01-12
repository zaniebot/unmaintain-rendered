```yaml
number: 5724
title: Workspace discovery fails on matching files
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
created_at: 2024-08-02T12:35:00Z
updated_at: 2024-08-02T19:44:44Z
url: https://github.com/astral-sh/uv/issues/5724
synced_at: 2026-01-12T15:58:58Z
```

# Workspace discovery fails on matching files

---

_@charliermarsh_

In Ruff:

```toml
[tool.uv.workspace]
members = ["docs/*", "python/ruff-ecosystem/*"]
```

```
❯ uv sync
warning: `uv sync` is experimental and may change without warning
error: failed to read from file `/home/micha/astral/ruff/docs/.gitignore/pyproject.toml`
  Caused by: Not a directory (os error 20)
```

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-02 12:35_

---

_Label `bug` added by @charliermarsh on 2024-08-02 12:35_

---

_Label `preview` added by @charliermarsh on 2024-08-02 12:35_

---

_Closed by @charliermarsh on 2024-08-02 19:44_

---

_Closed by @charliermarsh on 2024-08-02 19:44_

---
