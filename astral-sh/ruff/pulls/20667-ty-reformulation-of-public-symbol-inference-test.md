```yaml
number: 20667
title: "[ty] Reformulation of public symbol inference test suite"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/reformulate-public-symbol-test-suite
created_at: 2025-10-01T12:15:47Z
updated_at: 2025-10-01T12:26:20Z
url: https://github.com/astral-sh/ruff/pull/20667
synced_at: 2026-01-12T15:57:07Z
```

# [ty] Reformulation of public symbol inference test suite

---

_@sharkdp_

## Summary

Reformulation of the public symbol type inference test suite to use class scopes instead of module scopes. This is in preparation for an upcoming change to module-global scopes (#20664).

## Test Plan

Updated tests


---

_Review requested from @carljm by @sharkdp on 2025-10-01 12:15_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-01 12:15_

---

_Review requested from @dcreager by @sharkdp on 2025-10-01 12:15_

---

_Label `testing` added by @sharkdp on 2025-10-01 12:15_

---

_Label `ty` added by @sharkdp on 2025-10-01 12:15_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/boundness_declaredness/public.md`:248 on 2025-10-01 12:18_

This is the only behavioral change. It makes sense that we emit an error for class scopes. For the module-global scope, "modifying `a`" was just creating a new variable.

---

_@sharkdp reviewed on 2025-10-01 12:18_

---

_Merged by @sharkdp on 2025-10-01 12:26_

---

_Closed by @sharkdp on 2025-10-01 12:26_

---

_Branch deleted on 2025-10-01 12:26_

---
