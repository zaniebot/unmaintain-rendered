```yaml
number: 11137
title: "`ruff server`: In 'publish diagnostics' mode, document diagnostics are cleared properly when a file is closed"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/clear-diagnostics-on-close
created_at: 2024-04-25T01:51:35Z
updated_at: 2024-04-25T02:38:55Z
url: https://github.com/astral-sh/ruff/pull/11137
synced_at: 2026-01-10T22:37:01Z
```

# `ruff server`: In 'publish diagnostics' mode, document diagnostics are cleared properly when a file is closed

---

_Pull request opened by @snowsignal on 2024-04-25 01:51_

## Summary

Fixes #11114. 

As part of the `onClose` handler, we publish an empty array of diagnostics for the document being closed, similar to [`ruff-lsp`](https://github.com/astral-sh/ruff-lsp/blob/187d7790be0783b9ac41ce025a724cf389bf575c/ruff_lsp/server.py#L459-L464). This prevent phantom diagnostics from lingering after a document is closed. We'll only do this if the client doesn't support pull diagnostics, because otherwise clearing diagnostics is their responsibility.

## Test Plan

Diagnostics should no longer appear for a document in the Problems tab after the document is closed.


---

_Label `server` added by @snowsignal on 2024-04-25 01:51_

---

_@charliermarsh approved on 2024-04-25 02:02_

---

_Comment by @github-actions[bot] on 2024-04-25 02:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @snowsignal on 2024-04-25 02:38_

---

_Closed by @snowsignal on 2024-04-25 02:38_

---

_Branch deleted on 2024-04-25 02:38_

---
