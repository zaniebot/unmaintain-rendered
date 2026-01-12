```yaml
number: 20788
title: "[ty] Additional tests for `dataclass_transform` (class-level overwrites, `field_specifiers`)"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/new-tests-for-dataclass_transform
created_at: 2025-10-09T14:55:26Z
updated_at: 2025-10-10T11:22:07Z
url: https://github.com/astral-sh/ruff/pull/20788
synced_at: 2026-01-12T15:57:09Z
```

# [ty] Additional tests for `dataclass_transform` (class-level overwrites, `field_specifiers`)

---

_@sharkdp_

## Summary

Adds a set of basic new tests corresponding to open points in https://github.com/astral-sh/ty/issues/1327, to document the state of support for `dataclass_transform`.

---

_Review requested from @carljm by @sharkdp on 2025-10-09 14:55_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-09 14:55_

---

_Review requested from @dcreager by @sharkdp on 2025-10-09 14:55_

---

_Label `testing` added by @sharkdp on 2025-10-09 15:04_

---

_Label `ty` added by @sharkdp on 2025-10-09 15:04_

---

_Closed by @AlexWaygood on 2025-10-09 18:02_

---

_Reopened by @AlexWaygood on 2025-10-09 18:02_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:370 on 2025-10-10 11:11_

```suggestion
#### Using function-based transformers
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:392 on 2025-10-10 11:12_

the `dataclass_transform` decorator is applied _to_ a metaclass, but I would say that it is the "dataclass transformer" that is metaclass-based here

```suggestion
#### Using metaclass-based transformers
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:424 on 2025-10-10 11:14_

```suggestion
#### Using base-class-based transformers
```

---

_@AlexWaygood approved on 2025-10-10 11:16_

---

_Merged by @sharkdp on 2025-10-10 11:22_

---

_Closed by @sharkdp on 2025-10-10 11:22_

---

_Branch deleted on 2025-10-10 11:22_

---
