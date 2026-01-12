```yaml
number: 13531
title: "refactor: Rename `FormatStringContinuation` to `FormatImplicitConcatenatedString`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/rename-format-string-continuation
created_at: 2024-09-27T07:59:02Z
updated_at: 2024-09-27T08:30:33Z
url: https://github.com/astral-sh/ruff/pull/13531
synced_at: 2026-01-12T15:55:44Z
```

# refactor: Rename `FormatStringContinuation` to `FormatImplicitConcatenatedString`

---

_@MichaReiser_

## Summary

What's in the title. I don't think a `StringContinuation` is a proper term in Python. There's line continuation. 
Anyway, I think implicit concatenated string easier to understand.

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2024-09-27 07:59_

---

_Comment by @github-actions[bot] on 2024-09-27 08:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @MichaReiser on 2024-09-27 08:24_

---

_Closed by @MichaReiser on 2024-09-27 08:24_

---

_Branch deleted on 2024-09-27 08:24_

---
