```yaml
number: 16746
title: "[red-knot] Document current state of attribute assignment diagnostics"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/document-attribute-assignment-diagnostics
created_at: 2025-03-14T12:50:10Z
updated_at: 2025-03-14T19:34:45Z
url: https://github.com/astral-sh/ruff/pull/16746
synced_at: 2026-01-12T15:55:56Z
```

# [red-knot] Document current state of attribute assignment diagnostics

---

_@sharkdp_

## Summary

A follow-up to https://github.com/astral-sh/ruff/pull/16705 which documents various kinds of diagnostics that can appear when assigning to an attribute.

## Test Plan

New snapshot tests.

---

_Label `red-knot` added by @sharkdp on 2025-03-14 12:50_

---

_Review requested from @carljm by @sharkdp on 2025-03-14 12:50_

---

_Review requested from @MichaReiser by @sharkdp on 2025-03-14 12:50_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-03-14 12:50_

---

_Comment by @github-actions[bot] on 2025-03-14 12:56_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/snapshots/attribute_assignment.md_-_Attribute_assignment_-_Data_descriptors_-_Invalid_`__set__`_method_signature.snap`:36 on 2025-03-14 18:29_

Not for fixing in this PR, but just noting it seems like we are currently inconsistent about whether we blame an invalid-assignment error on the target, on the RHS, or on the entire expression. I'm seeing that in case of `x: int = "foo"` we mark the entire expression, in `x = "foo"` (with a separate prior `x: int`) we mark just the target, and now here in these attribute cases we also mark just the target. Pyright seems to be consistent about always marking the RHS, and I can see the justification for that.

---

_@carljm approved on 2025-03-14 18:30_

---

_@sharkdp reviewed on 2025-03-14 19:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/snapshots/attribute_assignment.md_-_Attribute_assignment_-_Data_descriptors_-_Invalid_`__set__`_method_signature.snap`:36 on 2025-03-14 19:34_

I also thought it would be more intuitive to highlight the RHS, but didn't want to change behavior in my last PR or here. But that should be easy to fix.

---

_Merged by @sharkdp on 2025-03-14 19:34_

---

_Closed by @sharkdp on 2025-03-14 19:34_

---

_Branch deleted on 2025-03-14 19:34_

---
