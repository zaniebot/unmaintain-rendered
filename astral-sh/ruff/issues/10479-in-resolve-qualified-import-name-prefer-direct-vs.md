```yaml
number: 10479
title: "In `resolve_qualified_import_name`, prefer direct vs. attribute names"
type: issue
state: open
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2024-03-19T18:02:55Z
updated_at: 2024-03-20T07:32:51Z
url: https://github.com/astral-sh/ruff/issues/10479
synced_at: 2026-01-12T15:54:50Z
```

# In `resolve_qualified_import_name`, prefer direct vs. attribute names

---

_@charliermarsh_

If both `import logging` and `from logging import error` are in scope, and the user asks for the symbol `logging.error`, I _think_ it makes sense to prefer `"error"` over `"logging.error"`.


---

_Label `internal` added by @charliermarsh on 2024-03-19 18:02_

---

_Comment by @dhruvmanila on 2024-03-20 07:32_

Yeah, I agree. If the symbol is already imported then we should use it directly instead.

---
