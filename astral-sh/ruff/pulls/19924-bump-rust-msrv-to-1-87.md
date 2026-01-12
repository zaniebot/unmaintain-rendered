```yaml
number: 19924
title: Bump Rust MSRV to 1.87
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: bump-msrv-1.87
created_at: 2025-08-15T07:40:01Z
updated_at: 2025-08-15T15:55:40Z
url: https://github.com/astral-sh/ruff/pull/19924
synced_at: 2026-01-12T15:56:50Z
```

# Bump Rust MSRV to 1.87

---

_@MichaReiser_

## Summary
Bumps the minimum supported Rust version from 1.86 to 1.87. This is in accordance with our MSRV policy N-2 where N is 1.89.

Related conda feedstock PR: https://github.com/conda-forge/ruff-feedstock/pull/290

## Test plan
- `cargo clippy` passes
- `cargo fmt` passes
- `cargo test` passes

---

_Label `release` added by @MichaReiser on 2025-08-15 07:40_

---

_Label `release` removed by @MichaReiser on 2025-08-15 07:40_

---

_Comment by @MichaReiser on 2025-08-15 07:41_

@oconnor663 1.87 stabilized `extract_if`

---

_Comment by @github-actions[bot] on 2025-08-15 07:52_

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

_@ntBre approved on 2025-08-15 15:53_

Nice! I think there's a nice use-case for `extract_if` in Ruff's noqa handling too.

---

_Merged by @MichaReiser on 2025-08-15 15:55_

---

_Closed by @MichaReiser on 2025-08-15 15:55_

---

_Branch deleted on 2025-08-15 15:55_

---
