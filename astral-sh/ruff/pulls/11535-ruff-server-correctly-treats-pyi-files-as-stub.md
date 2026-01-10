```yaml
number: 11535
title: "`ruff server` correctly treats `.pyi` files as stub files"
type: pull_request
state: merged
author: snowsignal
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: jane/server/pyi
created_at: 2024-05-24T21:17:32Z
updated_at: 2024-05-26T17:42:48Z
url: https://github.com/astral-sh/ruff/pull/11535
synced_at: 2026-01-10T22:05:26Z
```

# `ruff server` correctly treats `.pyi` files as stub files

---

_Pull request opened by @snowsignal on 2024-05-24 21:17_

## Summary

Fixes #11534.

`DocumentQuery::source_type` now returns `PySourceType::Stub` when the document is a `.pyi` file.

## Test Plan

I confirmed that stub-specific rule violations appeared with a build from this PR (they were not visible from a `main` build).

<img width="1066" alt="Screenshot 2024-05-24 at 2 15 38 PM" src="https://github.com/astral-sh/ruff/assets/19577865/cd519b7e-21e4-41c8-bc30-43eb6d4d438e">


---

_Label `bug` added by @snowsignal on 2024-05-24 21:17_

---

_Label `server` added by @snowsignal on 2024-05-24 21:17_

---

_Comment by @github-actions[bot] on 2024-05-24 21:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-26 17:31_

---

_@charliermarsh approved on 2024-05-26 17:42_

---

_Merged by @charliermarsh on 2024-05-26 17:42_

---

_Closed by @charliermarsh on 2024-05-26 17:42_

---

_Branch deleted on 2024-05-26 17:42_

---
