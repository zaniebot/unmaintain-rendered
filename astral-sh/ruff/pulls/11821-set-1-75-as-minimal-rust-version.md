```yaml
number: 11821
title: Set 1.75 as minimal Rust version
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: set-minimal-rust-to-175
created_at: 2024-06-10T06:30:01Z
updated_at: 2024-06-10T12:39:23Z
url: https://github.com/astral-sh/ruff/pull/11821
synced_at: 2026-01-12T15:55:39Z
```

# Set 1.75 as minimal Rust version

---

_@MichaReiser_

## Summary

Salsa requires https://github.com/rust-lang/rust/issues/91611 which stabalized with [1.75](https://blog.rust-lang.org/2023/12/28/Rust-1.75.0.html)

1.75 doesn't change the supported platforms. So I think this upgrade is fine in a minor.

## Test Plan

`cargo build`


---

_Label `internal` added by @MichaReiser on 2024-06-10 06:30_

---

_Label `internal` removed by @MichaReiser on 2024-06-10 06:30_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-06-10 06:30_

---

_Comment by @github-actions[bot] on 2024-06-10 06:49_

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

_@charliermarsh approved on 2024-06-10 12:39_

---

_Merged by @charliermarsh on 2024-06-10 12:39_

---

_Closed by @charliermarsh on 2024-06-10 12:39_

---

_Branch deleted on 2024-06-10 12:39_

---
