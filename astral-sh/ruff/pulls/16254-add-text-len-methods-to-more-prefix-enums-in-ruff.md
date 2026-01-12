```yaml
number: 16254
title: "Add `text_len()` methods to more `*Prefix` enums in `ruff_python_ast`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/const-textlen
created_at: 2025-02-19T14:43:31Z
updated_at: 2025-02-19T14:52:51Z
url: https://github.com/astral-sh/ruff/pull/16254
synced_at: 2026-01-12T15:55:54Z
```

# Add `text_len()` methods to more `*Prefix` enums in `ruff_python_ast`

---

_@AlexWaygood_

## Summary

https://github.com/astral-sh/ruff/pull/16183 added a `StringLiteralPrefix::text_len()` method. This PR adds similar methods to the other `*Prefix` methods, for a consistent API. This also allows us to remove a little bit of indirection from the `StringFlags::opener_len()` method.

## Test Plan

`cargo test`


---

_Label `internal` added by @AlexWaygood on 2025-02-19 14:43_

---

_Merged by @AlexWaygood on 2025-02-19 14:47_

---

_Closed by @AlexWaygood on 2025-02-19 14:47_

---

_Branch deleted on 2025-02-19 14:47_

---

_Comment by @github-actions[bot] on 2025-02-19 14:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
