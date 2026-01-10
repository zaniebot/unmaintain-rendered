```yaml
number: 10478
title: Avoid failures due to non-deterministic binding ordering
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/t
created_at: 2024-03-19T17:54:42Z
updated_at: 2024-03-19T18:07:56Z
url: https://github.com/astral-sh/ruff/pull/10478
synced_at: 2026-01-10T22:47:02Z
```

# Avoid failures due to non-deterministic binding ordering

---

_Pull request opened by @charliermarsh on 2024-03-19 17:54_

## Summary

We're seeing failures in https://github.com/astral-sh/ruff/issues/10470 because `resolve_qualified_import_name` isn't guaranteed to return a specific import if a symbol is accessible in two ways (e.g., you have both `import logging` and `from logging import error` in scope, and you want `logging.error`). This PR breaks up the failing tests such that the imports aren't in the same scope.

Closes https://github.com/astral-sh/ruff/issues/10470.

## Test Plan

I added a `bindings.reverse()` to `resolve_qualified_import_name` to ensure that the tests pass regardless of the binding order.


---

_Label `testing` added by @charliermarsh on 2024-03-19 17:54_

---

_Merged by @charliermarsh on 2024-03-19 18:01_

---

_Closed by @charliermarsh on 2024-03-19 18:01_

---

_Branch deleted on 2024-03-19 18:01_

---

_Comment by @charliermarsh on 2024-03-19 18:03_

Would also be solved by https://github.com/astral-sh/ruff/issues/10479.

---

_Comment by @github-actions[bot] on 2024-03-19 18:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
