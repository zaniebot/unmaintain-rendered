```yaml
number: 13837
title: Modernize build scripts
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: modernise-build-scripts
created_at: 2024-10-20T21:29:47Z
updated_at: 2024-10-20T21:43:25Z
url: https://github.com/astral-sh/ruff/pull/13837
synced_at: 2026-01-12T15:55:45Z
```

# Modernize build scripts

---

_@AlexWaygood_

## Summary

Use the modern `cargo::KEY=VALUE` syntax (as described in the [docs](https://doc.rust-lang.org/cargo/reference/build-scripts.html#outputs-of-the-build-script)) that was stabilised in MSRV 1.77, rather than the deprecated `cargo:KEY=VALUE` syntax.

## Test Plan

`cargo test`


---

_Label `internal` added by @AlexWaygood on 2024-10-20 21:29_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-20 21:29_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-20 21:29_

---

_@MichaReiser approved on 2024-10-20 21:34_

---

_Merged by @AlexWaygood on 2024-10-20 21:35_

---

_Closed by @AlexWaygood on 2024-10-20 21:35_

---

_Branch deleted on 2024-10-20 21:35_

---

_Comment by @github-actions[bot] on 2024-10-20 21:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
