```yaml
number: 12682
title: Fix module resolver symlink test on macOs
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: fix-module-resolver-symlink-test-on-macos
created_at: 2024-08-05T07:18:44Z
updated_at: 2024-08-05T07:32:01Z
url: https://github.com/astral-sh/ruff/pull/12682
synced_at: 2026-01-10T21:47:02Z
```

# Fix module resolver symlink test on macOs

---

_Pull request opened by @MichaReiser on 2024-08-05 07:18_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/12656

`tempdir` on macOs uses a symlink and https://github.com/astral-sh/ruff/pull/12634 started to canonicalize search paths. 
That's why we need to update this test to use the canonicalized path of the temp directory. 

## Test Plan

`cargo test -p red_knot_module_resolver` passes on my mac


---

_Review requested from @carljm by @MichaReiser on 2024-08-05 07:18_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-05 07:18_

---

_Label `internal` added by @MichaReiser on 2024-08-05 07:18_

---

_Merged by @MichaReiser on 2024-08-05 07:22_

---

_Closed by @MichaReiser on 2024-08-05 07:22_

---

_Branch deleted on 2024-08-05 07:22_

---

_Comment by @github-actions[bot] on 2024-08-05 07:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
