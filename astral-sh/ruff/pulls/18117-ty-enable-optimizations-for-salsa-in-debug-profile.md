```yaml
number: 18117
title: "[ty] Enable optimizations for salsa in debug profile"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/optimize-salsa-in-dev
created_at: 2025-05-15T09:42:43Z
updated_at: 2025-05-15T10:31:48Z
url: https://github.com/astral-sh/ruff/pull/18117
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Enable optimizations for salsa in debug profile

---

_Pull request opened by @MichaReiser on 2025-05-15 09:42_

## Summary

We use salsa a lot and it changes infrequently. This PR enables more aggressive optimizations for the salsa crate when using the debug profile.
This should give us faster test (and ty execution) at the cost of slightly longer compile times when compiling a new salsa version for the first time.

## Test Plan

This reduces the wall time for `cargo nextest run -p ty_python_semantic --test mdtest` from ~4s to ~3.3s on my machine


---

_Label `testing` added by @MichaReiser on 2025-05-15 09:42_

---

_Label `ty` added by @MichaReiser on 2025-05-15 09:42_

---

_Comment by @github-actions[bot] on 2025-05-15 09:54_

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

_@sharkdp approved on 2025-05-15 10:27_

Thank you!

---

_Merged by @MichaReiser on 2025-05-15 10:31_

---

_Closed by @MichaReiser on 2025-05-15 10:31_

---

_Branch deleted on 2025-05-15 10:31_

---
