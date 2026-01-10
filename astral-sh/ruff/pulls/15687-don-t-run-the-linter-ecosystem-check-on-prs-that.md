```yaml
number: 15687
title: "Don't run the linter ecosystem check on PRs that only touch red-knot crates"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/no-redknot-ecosystem
created_at: 2025-01-23T10:37:20Z
updated_at: 2025-01-23T11:01:31Z
url: https://github.com/astral-sh/ruff/pull/15687
synced_at: 2026-01-10T20:05:43Z
```

# Don't run the linter ecosystem check on PRs that only touch red-knot crates

---

_Pull request opened by @AlexWaygood on 2025-01-23 10:37_

## Summary

At some point we'll hopefully setup a `mypy_primer`-like check for red-knot. Until that point, however, comments like https://github.com/astral-sh/ruff/pull/15683#issuecomment-2609273004 just cause noise and distraction through additional notifications, and it's a waste of our CI minutes.

## Test Plan

I used https://codepen.io/mrmlnc/pen/OXQjMe to check that the glob patterns listed here will match a path like `crates/ruff_linter/foo.rs` but will not match `crates/red_knot/foo.rs` or `crates/red_knot_python_semantic/foo.rs`. https://codepen.io/mrmlnc/pen/OXQjMe is linked to from the README of https://github.com/tj-actions/changed-files/tree/v45/ as a tool you can use to test glob patterns passed as inputs to the `changed-files` GitHub action.

---

_Label `ci` added by @AlexWaygood on 2025-01-23 10:37_

---

_Label `red-knot` added by @AlexWaygood on 2025-01-23 10:43_

---

_@MichaReiser approved on 2025-01-23 10:45_

---

_Merged by @AlexWaygood on 2025-01-23 10:47_

---

_Closed by @AlexWaygood on 2025-01-23 10:47_

---

_Branch deleted on 2025-01-23 10:47_

---

_Comment by @github-actions[bot] on 2025-01-23 11:01_

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
