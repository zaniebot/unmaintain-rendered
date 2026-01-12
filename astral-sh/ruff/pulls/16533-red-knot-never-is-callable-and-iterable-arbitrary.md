```yaml
number: 16533
title: "[red-knot] Never is callable and iterable. Arbitrary attributes can be accessed."
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/never-is-callable
created_at: 2025-03-06T13:13:06Z
updated_at: 2025-03-06T15:59:20Z
url: https://github.com/astral-sh/ruff/pull/16533
synced_at: 2026-01-12T15:55:55Z
```

# [red-knot] Never is callable and iterable. Arbitrary attributes can be accessed.

---

_@sharkdp_

## Summary

- `Never` is callable
- `Never` is iterable
- Arbitrary attributes can be accessed on `Never`

Split out from #16416 that is going to be required.

## Test Plan

Tests for all properties above.


---

_Label `red-knot` added by @sharkdp on 2025-03-06 13:13_

---

_Review requested from @carljm by @sharkdp on 2025-03-06 13:13_

---

_Review requested from @MichaReiser by @sharkdp on 2025-03-06 13:13_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-03-06 13:13_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1212 on 2025-03-06 13:20_

Is it worth also explicitly testing that arbitrary attributes can be set without any errors?

---

_@AlexWaygood approved on 2025-03-06 13:23_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1212 on 2025-03-06 14:20_

> that arbitrary attributes can be set without any errors

Is that the case? Never can be assigned to anything, but not the other way around. So I would expect every attribute assignment to be an error, unless the rhs-type is `Never`.

---

_@sharkdp reviewed on 2025-03-06 14:20_

---

_@AlexWaygood reviewed on 2025-03-06 14:38_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1212 on 2025-03-06 14:38_

> unless the rhs-type is `Never`

Yes, it was this that I was thinking of

---

_@AlexWaygood reviewed on 2025-03-06 15:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1212 on 2025-03-06 15:01_

(It might be worth adding a test for it either way, if we don't currently have any)

---

_@carljm approved on 2025-03-06 15:16_

---

_Merged by @sharkdp on 2025-03-06 15:59_

---

_Closed by @sharkdp on 2025-03-06 15:59_

---

_Branch deleted on 2025-03-06 15:59_

---
