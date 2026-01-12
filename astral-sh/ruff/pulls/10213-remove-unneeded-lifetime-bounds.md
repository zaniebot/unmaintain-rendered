```yaml
number: 10213
title: Remove unneeded lifetime bounds
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: remove-unneeded-lifetime-bounds
created_at: 2024-03-03T18:05:05Z
updated_at: 2024-03-03T18:23:40Z
url: https://github.com/astral-sh/ruff/pull/10213
synced_at: 2026-01-12T15:55:31Z
```

# Remove unneeded lifetime bounds

---

_@MichaReiser_

## Summary

This PR removes the unneeded lifetime `'b` from many of our `Visitor` implementations. 

The lifetime is unneeded because it is only constraint by `'a`, so we can use `'a` directly.

## Test Plan

`cargo build`


---

_Label `internal` added by @MichaReiser on 2024-03-03 18:05_

---

_@MichaReiser reviewed on 2024-03-03 18:05_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/reuse_of_groupby_generator.rs`:106 on 2024-03-03 18:05_

This is a good example why it's unneded. The lifetime of `stmt` should have been `'b` but it doesn't matter because `'a` and `'b` are equivalent.

---

_Merged by @MichaReiser on 2024-03-03 18:12_

---

_Closed by @MichaReiser on 2024-03-03 18:12_

---

_Branch deleted on 2024-03-03 18:12_

---

_Comment by @github-actions[bot] on 2024-03-03 18:23_

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
