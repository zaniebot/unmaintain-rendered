```yaml
number: 16222
title: "[red-knot] Refactor `infer_chained_boolean_types` to have access to `TypeInferenceBuilder`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/infer-chained-boolean-types
created_at: 2025-02-18T08:52:37Z
updated_at: 2025-02-19T10:13:37Z
url: https://github.com/astral-sh/ruff/pull/16222
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Refactor `infer_chained_boolean_types` to have access to `TypeInferenceBuilder`

---

_@MichaReiser_

## Summary

This is a small refactor to align the signature of the binary and boolean comparison `infer_` methods
closer to the signature of all other `infer` methods by making them instance methods on `TypeInferenceBuilder`. 

Having access to the `TypeInferenceBuilder` allows us to e.g. emit diagnostics during type inference. 

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-02-18 08:52_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-18 08:52_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-18 08:52_

---

_Label `red-knot` added by @MichaReiser on 2025-02-18 08:52_

---

_@AlexWaygood approved on 2025-02-18 11:06_

---

_Merged by @MichaReiser on 2025-02-19 10:13_

---

_Closed by @MichaReiser on 2025-02-19 10:13_

---

_Branch deleted on 2025-02-19 10:13_

---
