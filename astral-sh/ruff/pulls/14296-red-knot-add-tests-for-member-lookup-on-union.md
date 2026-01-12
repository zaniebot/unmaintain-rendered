```yaml
number: 14296
title: "[red-knot] Add tests for member lookup on union types"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/member-lookup-on-unions
created_at: 2024-11-12T12:49:19Z
updated_at: 2024-11-12T13:12:00Z
url: https://github.com/astral-sh/ruff/pull/14296
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] Add tests for member lookup on union types

---

_@sharkdp_

## Summary

- Write tests for member lookups on union types
- Remove TODO comment

part of: #14022

## Test Plan

New MD tests

---

_Label `red-knot` added by @sharkdp on 2024-11-12 12:49_

---

_Review requested from @carljm by @sharkdp on 2024-11-12 12:49_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-12 12:49_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-12 12:49_

---

_@sharkdp reviewed on 2024-11-12 12:50_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1111 on 2024-11-12 12:50_

As the tests show, we *do* raise a diagnostic if `member_boundness` indicates potential unboundness:
```py
class C1:
    x = 1

class C2:
    if bool_instance():
        x = 2

class C3:
    x = 3

flag1 = bool_instance()
flag2 = bool_instance()

C = C1 if flag1 else C2 if flag2 else C3

# error: [possibly-unbound-attribute] "Attribute `x` on type `Literal[C1, C2, C3]` is possibly unbound"
reveal_type(C.x)  # revealed: Literal[1, 2, 3]
```

So this comment can be removed.

---

_@AlexWaygood approved on 2024-11-12 13:02_

Fantastic. I haven't verified that this was previously untested but I assume you have 😄

---

_Comment by @github-actions[bot] on 2024-11-12 13:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @sharkdp on 2024-11-12 13:11_

> I haven't verified that this was previously untested but I assume you have 😄

Always make your tests fail :smile:. Commenting out one of the two `possibly_unbound = true;` statements in `Type::member` will cause one of the new unit tests to fail, while it went unnoticed previously.

---

_Merged by @sharkdp on 2024-11-12 13:11_

---

_Closed by @sharkdp on 2024-11-12 13:11_

---

_Branch deleted on 2024-11-12 13:12_

---
