```yaml
number: 20743
title: "[ty] Fix tiny mistake in protocol tests"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/fix-protocol-tests-mistake
created_at: 2025-10-07T10:24:53Z
updated_at: 2025-10-07T11:58:36Z
url: https://github.com/astral-sh/ruff/pull/20743
synced_at: 2026-01-12T15:57:08Z
```

# [ty] Fix tiny mistake in protocol tests

---

_@sharkdp_

_No description provided._

---

_Label `testing` added by @sharkdp on 2025-10-07 10:24_

---

_Review requested from @carljm by @sharkdp on 2025-10-07 10:24_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-07 10:24_

---

_Review requested from @dcreager by @sharkdp on 2025-10-07 10:24_

---

_Label `ty` added by @sharkdp on 2025-10-07 10:24_

---

_@sharkdp reviewed on 2025-10-07 10:25_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/protocols.md`:971 on 2025-10-07 10:25_

The `y` attribute is annotated as `y: str`, so this is not fine.

---

_@MichaReiser approved on 2025-10-07 10:27_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:968 on 2025-10-07 10:33_

It doesn't matter much, but I think the point here was to demonstrate that, just like other classes, it's okay for the sole assignment of the `y` attribute to be outside `__init__`. The only difference is that, unlike other classes, it does _have_ to have a declaration in the class body (implicit instance attributes in protocols are not allowed)

```suggestion
```

---

_@AlexWaygood approved on 2025-10-07 10:34_

---

_Merged by @sharkdp on 2025-10-07 11:58_

---

_Closed by @sharkdp on 2025-10-07 11:58_

---

_Branch deleted on 2025-10-07 11:58_

---
