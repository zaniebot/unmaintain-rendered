```yaml
number: 11800
title: "Use real file path when available in `ruff server`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: charlie/ig
created_at: 2024-06-08T01:49:09Z
updated_at: 2024-06-08T05:48:54Z
url: https://github.com/astral-sh/ruff/pull/11800
synced_at: 2026-01-12T15:55:39Z
```

# Use real file path when available in `ruff server`

---

_@charliermarsh_

## Summary

As-is, we're using the URL path for all files, leading us to use paths like:

```
/c%3A/Users/crmar/workspace/fastapi/tests/main.py
```

This doesn't match against per-file ignores and other patterns in Ruff configuration.

This PR modifies the LSP to use the real file path if available, and the virtual file path if not.

Closes https://github.com/astral-sh/ruff/issues/11751.

## Test Plan

Ran the LSP on Windows. In the FastAPI repo, added:

```toml
[tool.ruff.lint.per-file-ignores]
"tests/**/*.py" = ["F401"]
```

And verified that an unused import was ignored in `tests` after this change, but not before.


---

_Review requested from @MichaReiser by @charliermarsh on 2024-06-08 01:58_

---

_Review requested from @snowsignal by @charliermarsh on 2024-06-08 01:58_

---

_Label `bug` added by @charliermarsh on 2024-06-08 01:58_

---

_Label `server` added by @charliermarsh on 2024-06-08 01:58_

---

_Comment by @github-actions[bot] on 2024-06-08 02:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@snowsignal approved on 2024-06-08 05:48_

---

_Merged by @snowsignal on 2024-06-08 05:48_

---

_Closed by @snowsignal on 2024-06-08 05:48_

---

_Branch deleted on 2024-06-08 05:48_

---
