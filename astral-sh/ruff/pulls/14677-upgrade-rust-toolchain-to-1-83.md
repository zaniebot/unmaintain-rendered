```yaml
number: 14677
title: Upgrade Rust toolchain to 1.83
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/rust-183
created_at: 2024-11-29T11:52:52Z
updated_at: 2024-11-29T12:09:53Z
url: https://github.com/astral-sh/ruff/pull/14677
synced_at: 2026-01-12T15:55:48Z
```

# Upgrade Rust toolchain to 1.83

---

_@MichaReiser_

## Summary

Upgrade the Rust Toolchain to 1.83.

This PR does not bump the MSRV.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-11-29 11:52_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-29 11:52_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-29 11:52_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-11-29 11:52_

---

_Label `internal` added by @MichaReiser on 2024-11-29 11:53_

---

_Review comment by @AlexWaygood on `crates/ruff_formatter/src/printer/mod.rs`:1052 on 2024-11-29 11:56_

what's this `impl` block even for?! It's completely empty

---

_@AlexWaygood approved on 2024-11-29 11:58_

---

_@MichaReiser reviewed on 2024-11-29 11:59_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/printer/mod.rs`:1052 on 2024-11-29 11:59_

It's decorative. 

---

_Merged by @MichaReiser on 2024-11-29 12:05_

---

_Closed by @MichaReiser on 2024-11-29 12:05_

---

_Branch deleted on 2024-11-29 12:05_

---

_Comment by @github-actions[bot] on 2024-11-29 12:09_

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
