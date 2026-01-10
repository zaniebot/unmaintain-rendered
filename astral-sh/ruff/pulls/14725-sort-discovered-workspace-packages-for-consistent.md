```yaml
number: 14725
title: Sort discovered workspace packages for consistent cross-platform package discovery
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/fix-unstable-workspace-test
created_at: 2024-12-02T07:31:47Z
updated_at: 2024-12-02T07:37:43Z
url: https://github.com/astral-sh/ruff/pull/14725
synced_at: 2026-01-10T20:42:27Z
```

# Sort discovered workspace packages for consistent cross-platform package discovery

---

_Pull request opened by @MichaReiser on 2024-12-02 07:31_

## Summary

Sort the packages discovered by `collect_packages` by path for 
consistent cross-platform package-ordering (instead of relying on globs ordering)

Fixes https://github.com/astral-sh/ruff/issues/14724

## Test Plan

`cargo test --target i686-pc-windows-msvc -p red_knot_workspace  -- workspace_non_unique_member_names`




---

_Review requested from @carljm by @MichaReiser on 2024-12-02 07:31_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-02 07:31_

---

_Review requested from @sharkdp by @MichaReiser on 2024-12-02 07:31_

---

_Label `internal` added by @MichaReiser on 2024-12-02 07:32_

---

_Label `internal` removed by @MichaReiser on 2024-12-02 07:32_

---

_Label `testing` added by @MichaReiser on 2024-12-02 07:32_

---

_Label `red-knot` added by @MichaReiser on 2024-12-02 07:32_

---

_Merged by @MichaReiser on 2024-12-02 07:36_

---

_Closed by @MichaReiser on 2024-12-02 07:36_

---

_Branch deleted on 2024-12-02 07:36_

---

_Comment by @github-actions[bot] on 2024-12-02 07:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
