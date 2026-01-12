```yaml
number: 14006
title: Fix panic when filling up types vector during unpacking
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/fix-unpack-panic
created_at: 2024-10-30T18:55:07Z
updated_at: 2024-10-30T19:23:03Z
url: https://github.com/astral-sh/ruff/pull/14006
synced_at: 2026-01-12T15:55:46Z
```

# Fix panic when filling up types vector during unpacking

---

_@dhruvmanila_

## Summary

This PR fixes a panic which can occur in an unpack assignment when:
* (number of target expressions) - (number of tuple types) > 2
* There's a starred expression

The reason being that the `insert` panics because the index is greater than the length.

This is an error case and so practically it should occur very rarely. The solution is to resize the types vector to match the number of expressions and then insert the starred expression type.

## Test Plan

Add a new test case.


---

_Label `red-knot` added by @dhruvmanila on 2024-10-30 18:55_

---

_Review requested from @carljm by @dhruvmanila on 2024-10-30 18:55_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-10-30 18:55_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-10-30 18:55_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1264 on 2024-10-30 18:56_

Hmm, I think I should replace here instead of insert.

---

_@dhruvmanila reviewed on 2024-10-30 18:56_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:142 on 2024-10-30 19:02_

Rebase on main to remove the bogus not-iterable error here?

---

_@carljm approved on 2024-10-30 19:06_

Nice!

---

_Merged by @dhruvmanila on 2024-10-30 19:13_

---

_Closed by @dhruvmanila on 2024-10-30 19:13_

---

_Branch deleted on 2024-10-30 19:13_

---

_Comment by @github-actions[bot] on 2024-10-30 19:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
