```yaml
number: 16958
title: "[red-knot] Fix panic on cyclic `*` imports"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/star-import-cycles
created_at: 2025-03-24T17:59:59Z
updated_at: 2025-03-24T18:24:59Z
url: https://github.com/astral-sh/ruff/pull/16958
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] Fix panic on cyclic `*` imports

---

_Pull request opened by @AlexWaygood on 2025-03-24 17:59_

## Summary

Further work towards https://github.com/astral-sh/ruff/issues/14169.

We currently panic on encountering cyclic `*` imports. This is easily fixed using fixpoint iteration.

## Test Plan

Added a test that panics on `main`, but passes with this PR


---

_Label `red-knot` added by @AlexWaygood on 2025-03-24 17:59_

---

_Review requested from @carljm by @AlexWaygood on 2025-03-24 18:00_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-03-24 18:00_

---

_Review requested from @dcreager by @AlexWaygood on 2025-03-24 18:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:44 on 2025-03-24 18:05_

These names are generally related to the query name, so e.g. `exported_cycle_recover` and `exported_cycle_initial`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:924 on 2025-03-24 18:06_

I think it would be worth actually defining some real name in one of these modules, and then having a third module `import *` from one of these, and assert that the real name shows up there.

---

_@carljm reviewed on 2025-03-24 18:06_

Nice, thanks!

---

_@AlexWaygood reviewed on 2025-03-24 18:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:44 on 2025-03-24 18:08_

ooops, sorry!

---

_Comment by @github-actions[bot] on 2025-03-24 18:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review requested from @carljm by @AlexWaygood on 2025-03-24 18:14_

---

_@carljm approved on 2025-03-24 18:17_

---

_Merged by @AlexWaygood on 2025-03-24 18:23_

---

_Closed by @AlexWaygood on 2025-03-24 18:23_

---

_Branch deleted on 2025-03-24 18:23_

---
