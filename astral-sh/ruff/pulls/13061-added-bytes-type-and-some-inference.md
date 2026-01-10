```yaml
number: 13061
title: Added bytes type and some inference
type: pull_request
state: merged
author: teofr
labels:
  - ty
assignees: []
merged: true
base: main
head: teofr/red_knot_bytes
created_at: 2024-08-22T16:42:51Z
updated_at: 2024-08-27T23:47:44Z
url: https://github.com/astral-sh/ruff/pull/13061
synced_at: 2026-01-10T21:38:32Z
```

# Added bytes type and some inference

---

_Pull request opened by @teofr on 2024-08-22 16:42_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR adds the `bytes` type to red-knot:
- Added the `bytes` type
- Added support for bytes literals
- Support for the `+` operator

Improves on astral-sh/ty#244 

Big TODO on supporting and normalizing r-prefixed bytestrings (`rb"hello\n"`)

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Added a test for a bytes literals, concatenation, and corner values

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-08-22 16:56_

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

_Renamed from "[WIP] Added bytes type and some inference" to "Added bytes type and some inference" by @teofr on 2024-08-22 19:45_

---

_Marked ready for review by @teofr on 2024-08-22 19:45_

---

_Review requested from @carljm by @teofr on 2024-08-22 19:45_

---

_Review requested from @MichaReiser by @teofr on 2024-08-22 19:45_

---

_Review requested from @AlexWaygood by @teofr on 2024-08-22 19:45_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:282 on 2024-08-22 19:48_

We will probably want to just defer to `Type::Instance(<bytes from typeshed>).member` here, you could update the TODO but it's not critical

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1729 on 2024-08-22 19:49_

It occurs to me now that it might be nicer to flatten this out by matching on a `(lhs, rhs, op)` tuple instead, but that would change the existing `IntLiteral` code and is out of scope for this PR

---

_@carljm approved on 2024-08-22 19:51_

This is fantastic, thank you!!

---

_Merged by @carljm on 2024-08-22 20:27_

---

_Closed by @carljm on 2024-08-22 20:27_

---

_Branch deleted on 2024-08-27 21:55_

---

_Label `red-knot` added by @carljm on 2024-08-27 23:47_

---
