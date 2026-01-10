```yaml
number: 11493
title: "`ruff.applyFormat` now formats an entire notebook document"
type: pull_request
state: merged
author: snowsignal
labels:
  - notebook
  - server
assignees: []
merged: true
base: main
head: jane/server/notebook/format/all-cells-cmd
created_at: 2024-05-22T09:32:23Z
updated_at: 2024-05-27T10:41:06Z
url: https://github.com/astral-sh/ruff/pull/11493
synced_at: 2026-01-10T22:05:26Z
```

# `ruff.applyFormat` now formats an entire notebook document

---

_Pull request opened by @snowsignal on 2024-05-22 09:32_

## Summary

Previously, `ruff.applyFormat`, seen in VS Code as the command `Ruff: Format Document`, would only format the currently active notebook cell inside a notebook document. This PR makes `ruff.applyFormat` format the entire notebook document at once, operating on each code cell in order.

## Test Plan

1. Open a notebook document that has multiple unformatted code cells.
2. Run `Ruff: Format Document` through the Command Palette (`Ctrl/Cmd+Shift+P` by default)
3. Observe that all code cells in the notebook have been formatted.


---

_Label `notebook` added by @snowsignal on 2024-05-22 09:32_

---

_Label `server` added by @snowsignal on 2024-05-22 09:32_

---

_Comment by @github-actions[bot] on 2024-05-22 09:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila approved on 2024-05-22 11:13_

---

_Merged by @snowsignal on 2024-05-22 16:02_

---

_Closed by @snowsignal on 2024-05-22 16:02_

---

_Branch deleted on 2024-05-22 16:02_

---

_@MichaReiser reviewed on 2024-05-27 10:41_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/format.rs`:34 on 2024-05-27 10:41_

Nit: Would it make sense to store the result of  `snapshot.query()` in a variable. It's used in multiple places and feels fairly repetitive. 

---
