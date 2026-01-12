```yaml
number: 17907
title: Add check for missing imports
type: issue
state: closed
author: pierrefrojd
labels: []
assignees: []
created_at: 2025-05-07T06:38:06Z
updated_at: 2025-05-07T06:41:36Z
url: https://github.com/astral-sh/ruff/issues/17907
synced_at: 2026-01-12T15:54:56Z
```

# Add check for missing imports

---

_@pierrefrojd_

### Summary

My VSC setup has both Pylance and Ruff enabled. Pylance finds issues in the import statements, e.g., when imports are missing with "reportMissingImports", but Ruff don't.

Can this check be added to Ruff as well?

---

_Comment by @MichaReiser on 2025-05-07 06:41_

We'll probably implement a feature like this for our new type checker ty but I don't see us adding it to ruff in the near future because it requires a module resolver to only suggest module names that exist

---

_Closed by @MichaReiser on 2025-05-07 06:41_

---
