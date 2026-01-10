```yaml
number: 9438
title: "Move `locate_cmp_ops` to `invalid_literal_comparisons`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: move-locate-cmp-ops
created_at: 2024-01-08T11:18:56Z
updated_at: 2024-01-08T12:15:37Z
url: https://github.com/astral-sh/ruff/pull/9438
synced_at: 2026-01-10T22:57:09Z
```

# Move `locate_cmp_ops` to `invalid_literal_comparisons`

---

_Pull request opened by @MichaReiser on 2024-01-08 11:18_

## Summary

This PR removes the `locate_cmp_ops` function from the `rust_python_parser` and moves it to the `invalid_literal_comparison` rule instead (the only rule using the function). 

This helps to keep the parser API as narrow as possible.

## Test Plan

`cargo test`


---

_Review requested from @charliermarsh by @MichaReiser on 2024-01-08 11:19_

---

_Label `internal` added by @MichaReiser on 2024-01-08 11:19_

---

_Comment by @github-actions[bot] on 2024-01-08 11:37_

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

_Merged by @MichaReiser on 2024-01-08 12:15_

---

_Closed by @MichaReiser on 2024-01-08 12:15_

---

_Branch deleted on 2024-01-08 12:15_

---
