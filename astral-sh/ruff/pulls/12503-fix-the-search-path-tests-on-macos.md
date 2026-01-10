```yaml
number: 12503
title: Fix the search path tests on MacOS
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-search-path-tests-on-mac
created_at: 2024-07-25T06:06:44Z
updated_at: 2024-08-02T16:02:42Z
url: https://github.com/astral-sh/ruff/pull/12503
synced_at: 2026-01-10T21:47:02Z
```

# Fix the search path tests on MacOS

---

_Pull request opened by @MichaReiser on 2024-07-25 06:06_

## Summary

`tempfile` on MacOS uses some symlink magic which confuses the watcher. 

We should resolve symlinks when resolving the search paths. But for now, normalize the paths in the test.

## Test Plan

`cargo test` on MacOs` passes


---

_Review requested from @carljm by @MichaReiser on 2024-07-25 06:06_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-25 06:06_

---

_Comment by @github-actions[bot] on 2024-07-25 06:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @MichaReiser on 2024-07-25 06:21_

---

_Merged by @MichaReiser on 2024-07-25 06:21_

---

_Closed by @MichaReiser on 2024-07-25 06:21_

---

_Branch deleted on 2024-07-25 06:21_

---
