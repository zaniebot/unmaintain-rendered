```yaml
number: 17581
title: "[red-knot] Make `BoundMethodType` a salsa interned"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/bound-method-interned
created_at: 2025-04-23T12:57:25Z
updated_at: 2025-04-23T13:11:22Z
url: https://github.com/astral-sh/ruff/pull/17581
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Make `BoundMethodType` a salsa interned

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/17579

Use a salsa-interned instead of a salsa-tracked because we want that two `BoundMethodType`'s compare equal if they belong to the same function and self instance.

This fixes the property tests because salsa-trackeds can't be created outside of queries because their "instance" is owned by the creating query. 
The property tests started creating `BoundMethodType` instances in https://github.com/astral-sh/ruff/commit/e45f23b0ec1efd093c6588f9b7d56bf87c4eebae because
`is_subtype_of` now calls `Class::into_callable` which calls `into_bound_method_type`.

## Test Plan

I ran the property tests locally


---

_Review requested from @carljm by @MichaReiser on 2025-04-23 12:57_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-23 12:57_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-23 12:57_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-23 12:57_

---

_Label `red-knot` added by @MichaReiser on 2025-04-23 12:57_

---

_Comment by @github-actions[bot] on 2025-04-23 13:01_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@dcreager approved on 2025-04-23 13:07_

---

_Merged by @MichaReiser on 2025-04-23 13:11_

---

_Closed by @MichaReiser on 2025-04-23 13:11_

---

_Branch deleted on 2025-04-23 13:11_

---
