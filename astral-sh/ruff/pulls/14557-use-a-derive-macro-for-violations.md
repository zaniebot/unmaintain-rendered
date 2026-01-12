```yaml
number: 14557
title: Use a derive macro for Violations
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/violation-derive-macro
created_at: 2024-11-23T16:28:48Z
updated_at: 2024-11-27T09:43:59Z
url: https://github.com/astral-sh/ruff/pull/14557
synced_at: 2026-01-12T15:55:48Z
```

# Use a derive macro for Violations

---

_@MichaReiser_

## Summary

Use a derive macro instead of a custom proc macro to implement the auto generated `Violation` methods. 

Derive macros are easier to understand because they implement a trait with the same name. 
It also helps Rustc reasoning about the code. E.g. it now detected that some fields are unused and that all violations only need to be `pub(crate)`. Clippy now also lints our doc comments which it didn't do before.

I generated the documentation locally and verified that rules still have a documentation ;)

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2024-11-23 16:29_

---

_Comment by @github-actions[bot] on 2024-11-23 16:39_

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

_@MichaReiser reviewed on 2024-11-23 16:57_

---

_Review comment by @MichaReiser on `clippy.toml`:2 on 2024-11-23 16:57_

An indent of 4 is consistent with what we use in `cargo.toml`

---

_Comment by @charliermarsh on 2024-11-23 16:59_

Oh, this is awesome!

---

_Marked ready for review by @MichaReiser on 2024-11-23 17:59_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-23 17:59_

---

_@AlexWaygood reviewed on 2024-11-23 18:09_

---

_Review comment by @AlexWaygood on `crates/ruff_diagnostics/src/violation.rs`:25 on 2024-11-23 18:09_

```suggestion
    /// Returns an explanation of what this violation catches,
```

---

_Merged by @MichaReiser on 2024-11-27 09:41_

---

_Closed by @MichaReiser on 2024-11-27 09:41_

---

_Branch deleted on 2024-11-27 09:41_

---
