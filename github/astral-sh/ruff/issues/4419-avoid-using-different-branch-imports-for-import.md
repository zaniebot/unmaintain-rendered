---
number: 4419
title: Avoid using different-branch imports for import-dependent autofixes
type: issue
state: open
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-05-13T18:05:44Z
updated_at: 2023-05-13T18:05:44Z
url: https://github.com/astral-sh/ruff/issues/4419
synced_at: 2026-01-07T13:12:14-06:00
---

# Avoid using different-branch imports for import-dependent autofixes

---

_Issue opened by @charliermarsh on 2023-05-13 18:05_

If a binding is created within a conditional, we need to avoid using it later on for autofixes -- e.g., here, we can't use `sys.exit()` based on this import:

```py
if False:
    import sys
```

There are a few ways to solve this:

1. Store a `CONDITIONAL` flag on `Binding`, and just check that the binding didn't occur in _some_ conditional branch. If it did, just don't use it, skip the fix.
2. Use our `branch_detection` logic to see if this binding is in a different branch than the current expression.


---

_Label `bug` added by @charliermarsh on 2023-05-13 18:05_

---
