```yaml
number: 15078
title: "`type: ignore[codes]` and `knot: ignore`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/ignore-codes
created_at: 2024-12-20T10:23:47Z
updated_at: 2024-12-23T09:52:46Z
url: https://github.com/astral-sh/ruff/pull/15078
synced_at: 2026-01-10T20:42:27Z
```

# `type: ignore[codes]` and `knot: ignore`

---

_Pull request opened by @MichaReiser on 2024-12-20 10:23_

## Summary

This PR adds support for parsing `type: ignore` comments that have an optional codes segment, e.g. `type: ignore[a, b, c]`. It also introduces the new `knot: ignore[codes]` suppression comment. 

The logic is split into two parts: 

* A suppression comment parser that parses a single `# type: ignore` comment 
* The logic that creates one or multiple `Suppressions` for each comment. 


I've decided to "flatten" ignore comments with multiple codes into multiple `Suppression` instances because I think it will simplify
the tracking of unused suppressions and it avoids a nested vector for the lint ids in `Suppression`.  

## Test Plan

Added md test


---

_Label `red-knot` added by @MichaReiser on 2024-12-20 10:24_

---

_Comment by @github-actions[bot] on 2024-12-20 10:54_

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

_Marked ready for review by @MichaReiser on 2024-12-20 10:56_

---

_Review requested from @carljm by @MichaReiser on 2024-12-20 10:56_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-20 10:56_

---

_Review requested from @sharkdp by @MichaReiser on 2024-12-20 10:56_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/suppressions/knot-ignore.md`:115 on 2024-12-22 16:14_

Is this something that we should flag, or just let it pass silently?

---

_@carljm approved on 2024-12-22 16:22_

Sweet!

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/suppression.rs`:295 on 2024-12-23 05:11_

Should we use `is_python_whitespace`?

---

_@dhruvmanila reviewed on 2024-12-23 05:11_

---

_@MichaReiser reviewed on 2024-12-23 09:51_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/suppressions/knot-ignore.md`:115 on 2024-12-23 09:51_

Nice catch. I updated https://github.com/astral-sh/ruff/pull/15084 to correctly mark the suppression as unused

---

_@MichaReiser reviewed on 2024-12-23 09:52_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/suppression.rs`:295 on 2024-12-23 09:52_

Using `is_whitespace` should be fine because we're in a comment. This also matches the formatter's docstring handling

---

_Merged by @MichaReiser on 2024-12-23 09:52_

---

_Closed by @MichaReiser on 2024-12-23 09:52_

---

_Branch deleted on 2024-12-23 09:52_

---
