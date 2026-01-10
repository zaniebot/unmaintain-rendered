```yaml
number: 18371
title: "Add `offset` method to `ruff_python_trivia::Cursor`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/common-cursor-methods
created_at: 2025-05-29T14:14:02Z
updated_at: 2025-05-29T15:08:17Z
url: https://github.com/astral-sh/ruff/pull/18371
synced_at: 2026-01-10T18:45:04Z
```

# Add `offset` method to `ruff_python_trivia::Cursor`

---

_Pull request opened by @AlexWaygood on 2025-05-29 14:14_

## Summary

Two `Parser` structs in the `ty_test` crate have `offset()` and `skip_whitespace` methods that are identical in their implementations. These methods feel like they could live just as well on the `ruff_python_trivia` crate's `Cursor` struct, which would reduce code duplication. (Edit: following code review, this PR now _only_ adds the `offset` method to the `Cursor` struct.)

The short-term motivation for doing this is that I found myself needing them yet again for another parsing task in https://github.com/astral-sh/ruff/pull/18057 -- but it seems like a reasonable thing to do anyway, so I'm splitting it out into a standalone PR.

## Test Plan

`cargo test -p ty_test`


---

_Review requested from @carljm by @AlexWaygood on 2025-05-29 14:14_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-05-29 14:14_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-29 14:14_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-29 14:14_

---

_Label `internal` added by @AlexWaygood on 2025-05-29 14:14_

---

_@MichaReiser reviewed on 2025-05-29 14:16_

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/cursor.rs`:175 on 2025-05-29 14:16_

I prefer to not have this method on cursor in `ruff_python_trivia` because python has different whitespace semantics (it only accepts `' '`, `\t` and `\f` and newlines). See `is_python_whitespace`. 

If we have this method here, at least make it very clear that it isn't python whitespace

---

_Comment by @github-actions[bot] on 2025-05-29 14:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-05-29 14:23_

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

_Renamed from "Move common parsing methods to the `Cursor` struct" to "Add `offset` method to `ruff_python_trivia::Cursor`'" by @AlexWaygood on 2025-05-29 14:46_

---

_Renamed from "Add `offset` method to `ruff_python_trivia::Cursor`'" to "Add `offset` method to `ruff_python_trivia::Cursor`" by @AlexWaygood on 2025-05-29 14:46_

---

_Review comment by @AlexWaygood on `crates/ruff_python_trivia/src/cursor.rs`:175 on 2025-05-29 14:46_

Thanks, I reverted this -- agree that this would have been a footgun!

---

_@AlexWaygood reviewed on 2025-05-29 14:46_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-05-29 14:46_

---

_@MichaReiser approved on 2025-05-29 15:07_

ty

---

_Merged by @AlexWaygood on 2025-05-29 15:08_

---

_Closed by @AlexWaygood on 2025-05-29 15:08_

---

_Branch deleted on 2025-05-29 15:08_

---
