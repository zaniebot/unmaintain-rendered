```yaml
number: 17603
title: "[red-knot] Add more tests for protocols"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/more-protocol-tests
created_at: 2025-04-24T11:06:20Z
updated_at: 2025-04-24T12:11:33Z
url: https://github.com/astral-sh/ruff/pull/17603
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Add more tests for protocols

---

_@AlexWaygood_

## Summary

Add tests covering:
- When a protocol should be considered fully static
- When protocol instances should be considered to be always truthy, always falsy or ambiguously truthy
- An edge case to do with validation of protocol members: it's okay for a protocol class to have an attribute assigned to without a declaration in that class _if_ the attribute is declared in a superclass of the protocol class

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `testing` added by @AlexWaygood on 2025-04-24 11:06_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-24 11:06_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-24 11:06_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-24 11:06_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-24 11:06_

---

_Comment by @github-actions[bot] on 2025-04-24 11:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-04-24 12:10_

---

_Merged by @AlexWaygood on 2025-04-24 12:11_

---

_Closed by @AlexWaygood on 2025-04-24 12:11_

---

_Branch deleted on 2025-04-24 12:11_

---
