```yaml
number: 13920
title: "Enable nursery rules: 'redundant_clone', 'debug_assert_with_mut_call', and 'unused_peekable'"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/more-clippy
created_at: 2024-10-25T07:08:21Z
updated_at: 2024-10-25T07:46:32Z
url: https://github.com/astral-sh/ruff/pull/13920
synced_at: 2026-01-10T20:59:37Z
```

# Enable nursery rules: 'redundant_clone', 'debug_assert_with_mut_call', and 'unused_peekable'

---

_Pull request opened by @MichaReiser on 2024-10-25 07:08_

## Summary

This PR makes clippy more pedantic ;) 

It enables three more clippy rules that capture code patterns that caused bugs in the past or that I noticed during code reviews:

* `redundant_clone`: It's easy to needlessly clone a value and this has come up in reviews. 
* `debug_assert_with_mut_call`: A first version of red knot contained debug asserts with mut calls and it broke downstream packaging because some of them ran our tests in release mode. This has also come up in contributions where it was unclear why the benchmark failed but all tests pass.
* `unused_peekable`: It's so easy to forget removing the `.peekable` call

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-10-25 07:08_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-25 07:08_

---

_Converted to draft by @MichaReiser on 2024-10-25 07:08_

---

_Label `internal` added by @MichaReiser on 2024-10-25 07:10_

---

_Marked ready for review by @MichaReiser on 2024-10-25 07:10_

---

_Comment by @github-actions[bot] on 2024-10-25 07:27_

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

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/util/subscript.rs`:21 on 2024-10-25 07:41_

Why are we allowing it for this module? Could you possibly add a comment?

---

_@AlexWaygood approved on 2024-10-25 07:42_

Nice!

---

_@MichaReiser reviewed on 2024-10-25 07:46_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/util/subscript.rs`:21 on 2024-10-25 07:46_

It's just tests and removing the clone would make adding a new case more verbose. I think it's very self-explanatory if you remove the allow, and I don't mind if someone's eager to fix the unnecessary calls. It just didn't felt worth my time. 

---

_Merged by @MichaReiser on 2024-10-25 07:46_

---

_Closed by @MichaReiser on 2024-10-25 07:46_

---

_Branch deleted on 2024-10-25 07:46_

---
