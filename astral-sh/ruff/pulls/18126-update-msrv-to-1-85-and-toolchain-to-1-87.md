```yaml
number: 18126
title: Update MSRV to 1.85 and toolchain to 1.87
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/rust-187
created_at: 2025-05-16T06:35:01Z
updated_at: 2025-05-16T07:19:57Z
url: https://github.com/astral-sh/ruff/pull/18126
synced_at: 2026-01-10T18:51:01Z
```

# Update MSRV to 1.85 and toolchain to 1.87

---

_Pull request opened by @MichaReiser on 2025-05-16 06:35_

## Summary

This PR updates the MSRV to 1.85 (according to our versioning policy) and the development Rust version to 1.87 (latest).

This PR does not switch to the Rust 2024 edition. I think it's better if we do this in a separate PR

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-05-16 06:35_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-16 06:35_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-16 06:35_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-16 06:35_

---

_Label `internal` added by @MichaReiser on 2025-05-16 06:35_

---

_@MichaReiser reviewed on 2025-05-16 06:36_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/source.rs`:136 on 2025-05-16 06:36_

Notebooks are massive and clippy complaint that the enum variants where significantly different in size (24 vs > 500)

---

_Comment by @github-actions[bot] on 2025-05-16 06:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-05-16 06:49_

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

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:5037 on 2025-05-16 06:54_

Why was this removed?

---

_@sharkdp approved on 2025-05-16 06:55_

Thank you.

---

_@MichaReiser reviewed on 2025-05-16 07:03_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:5037 on 2025-05-16 07:03_

Clippy told me that `#[must_use]` on traits are useless :( 

---

_Merged by @MichaReiser on 2025-05-16 07:19_

---

_Closed by @MichaReiser on 2025-05-16 07:19_

---

_Branch deleted on 2025-05-16 07:19_

---
