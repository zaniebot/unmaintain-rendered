```yaml
number: 13816
title: Upgrade to Rust 1.82
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/rust-1.82
created_at: 2024-10-19T10:08:10Z
updated_at: 2024-10-19T16:03:08Z
url: https://github.com/astral-sh/ruff/pull/13816
synced_at: 2026-01-12T15:55:45Z
```

# Upgrade to Rust 1.82

---

_@MichaReiser_

## Summary

Upgrade the developer toolchain. This does not bump the minimal MSRV.

## Test Plan

`cargo test`

codspeed now reports a 1% regression which is in the error-margin.


---

_Review requested from @carljm by @MichaReiser on 2024-10-19 10:08_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-19 10:08_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-10-19 10:08_

---

_Label `internal` added by @MichaReiser on 2024-10-19 10:08_

---

_Comment by @github-actions[bot] on 2024-10-19 10:43_

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

_Label `performance` added by @MichaReiser on 2024-10-19 11:32_

---

_Label `parser` added by @MichaReiser on 2024-10-19 11:32_

---

_Label `performance` removed by @MichaReiser on 2024-10-19 11:35_

---

_Label `parser` removed by @MichaReiser on 2024-10-19 11:35_

---

_@zanieb approved on 2024-10-19 13:03_

---

_Merged by @MichaReiser on 2024-10-19 14:05_

---

_Closed by @MichaReiser on 2024-10-19 14:05_

---

_Branch deleted on 2024-10-19 14:05_

---

_Comment by @carljm on 2024-10-19 15:46_

Do we know what caused the larger regression in the previous version of this diff?

---

_Comment by @AlexWaygood on 2024-10-19 16:03_

> Do we know what caused the larger regression in the previous version of this diff?

It looks like @MichaReiser fixed those issues in https://github.com/astral-sh/ruff/pull/13815

---
