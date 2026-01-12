```yaml
number: 12022
title: "[red-knot] Fix tests in release builds"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-red-knot-tests-in-release
created_at: 2024-06-25T06:30:34Z
updated_at: 2024-06-25T14:06:42Z
url: https://github.com/astral-sh/ruff/pull/12022
synced_at: 2026-01-12T15:55:40Z
```

# [red-knot] Fix tests in release builds

---

_@MichaReiser_

## Summary


This PR fixes the red-knot tests when run in release mode. 

The problem was that the semantic indexer code contained code with side effects inside of `debug_assert_eq` calls, that get stripped (entirely)
from release builds, removing the side effects. 

This PR moves the side-effects out from the `debug_assert_eq` calls.

Fixes https://github.com/astral-sh/ruff/issues/12018

## Test Plan

`cargo test -p red_knot --release`


---

_Review requested from @carljm by @MichaReiser on 2024-06-25 06:30_

---

_Label `red-knot` added by @MichaReiser on 2024-06-25 06:30_

---

_Merged by @MichaReiser on 2024-06-25 06:34_

---

_Closed by @MichaReiser on 2024-06-25 06:34_

---

_Branch deleted on 2024-06-25 06:34_

---

_Comment by @github-actions[bot] on 2024-06-25 06:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @carljm on 2024-06-25 14:02_

Thanks!

---
