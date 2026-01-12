```yaml
number: 13326
title: Add support for extensionless Python files for server
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
assignees: []
merged: true
base: main
head: dhruv/server-language-id
created_at: 2024-09-11T15:33:12Z
updated_at: 2024-09-11T19:05:29Z
url: https://github.com/astral-sh/ruff/pull/13326
synced_at: 2026-01-12T15:55:43Z
```

# Add support for extensionless Python files for server

---

_@dhruvmanila_

## Summary

Closes: #12539 

## Test Plan

https://github.com/user-attachments/assets/e49b2669-6f12-4684-9e45-a3321b19b659



---

_Label `server` added by @dhruvmanila on 2024-09-11 15:33_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-09-11 15:33_

---

_Renamed from "Add support for extensionless Python file for server" to "Add support for extensionless Python files for server" by @dhruvmanila on 2024-09-11 15:33_

---

_Comment by @github-actions[bot] on 2024-09-11 15:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-09-11 18:53_

Thanks. 

Checking all recognized python files seems like a sensible thing to me. My only concern is that the LSP and CLI now disagree on the file that need checking. 

---

_Comment by @dhruvmanila on 2024-09-11 19:04_

> Checking all recognized python files seems like a sensible thing to me. My only concern is that the LSP and CLI now disagree on the file that need checking.

That's true but this behavior seems reasonable from an editor perspective because the client provides us this information. This is also a regression from `ruff-lsp` which is resolved with this PR.

---

_Merged by @dhruvmanila on 2024-09-11 19:05_

---

_Closed by @dhruvmanila on 2024-09-11 19:05_

---

_Branch deleted on 2024-09-11 19:05_

---
