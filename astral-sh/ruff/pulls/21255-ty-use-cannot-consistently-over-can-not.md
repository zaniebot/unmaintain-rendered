```yaml
number: 21255
title: "[ty] Use \"cannot\" consistently over \"can not\""
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/cannot
created_at: 2025-11-03T15:05:53Z
updated_at: 2025-11-03T15:38:23Z
url: https://github.com/astral-sh/ruff/pull/21255
synced_at: 2026-01-12T15:57:19Z
```

# [ty] Use "cannot" consistently over "can not"

---

_@AlexWaygood_

## Summary

This is mostly an internal change, but does impact a few ty diagnostics

## Test Plan

`cargo test`


---

_Review requested from @carljm by @AlexWaygood on 2025-11-03 15:05_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-03 15:05_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-03 15:05_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-11-03 15:05_

---

_Label `ty` added by @AlexWaygood on 2025-11-03 15:05_

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-03 15:05_

---

_Comment by @github-actions[bot] on 2025-11-03 15:09_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-03 15:09_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/datastructures/auth.py:205:9: error[unresolved-attribute] Can not assign object of type `CallbackDict[Unknown, Unknown]` to attribute `_parameters` on type `Self@parameters` with custom `__setattr__` method.
+ src/werkzeug/datastructures/auth.py:205:9: error[unresolved-attribute] Cannot assign object of type `CallbackDict[Unknown, Unknown]` to attribute `_parameters` on type `Self@parameters` with custom `__setattr__` method.

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/test_config.py:69:5: error[unresolved-attribute] Can not assign object of type `float` to attribute `chop_threshold` on type `Display` with custom `__setattr__` method.
+ tests/test_config.py:69:5: error[unresolved-attribute] Cannot assign object of type `float` to attribute `chop_threshold` on type `Display` with custom `__setattr__` method.

```
</details>
No memory usage changes detected ✅


---

_@MichaReiser reviewed on 2025-11-03 15:10_

As a non native speaker: Why prefer "cannot" over "can not". We should probably also start writing down some guidance on how to write diagnostic messages

---

_Comment by @github-actions[bot] on 2025-11-03 15:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @AlexWaygood on 2025-11-03 15:32_

> As a non native speaker: Why prefer "cannot" over "can not". We should probably also start writing down some guidance on how to write diagnostic messages

There's not any difference in meaning, but "cannot" is far more commonly used:
- https://www.merriam-webster.com/grammar/cannot-vs-can-not-is-there-a-difference
- https://www.grammarly.com/blog/commonly-confused-words/cannot-or-can-not/
- https://www.microsoft.com/en-us/microsoft-365-life-hacks/writing/when-to-use-cannot-vs-can-not

As a result, "can not" ends up looking odd to my eyes: I trip up slightly every time I read it. You should basically always use "cannot" rather than "can not", I think

---

_@MichaReiser approved on 2025-11-03 15:34_

---

_Merged by @AlexWaygood on 2025-11-03 15:38_

---

_Closed by @AlexWaygood on 2025-11-03 15:38_

---

_Branch deleted on 2025-11-03 15:38_

---
