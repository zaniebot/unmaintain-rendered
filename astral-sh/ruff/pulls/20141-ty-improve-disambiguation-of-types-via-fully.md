```yaml
number: 20141
title: "[ty] Improve disambiguation of types via fully qualified names"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/return-fully-qualified
created_at: 2025-08-28T19:46:50Z
updated_at: 2025-08-29T08:45:10Z
url: https://github.com/astral-sh/ruff/pull/20141
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Improve disambiguation of types via fully qualified names

---

_Pull request opened by @AlexWaygood on 2025-08-28 19:46_

## Summary

Some small followups to #19850:
- Disambiguate classes (where appropriate) in `invalid-return-type` as well as `invalid-assignment`
- Extend the disambiguation to work with the `Display` of generic aliases

## Test Plan

Snapshot added


---

_Review requested from @carljm by @AlexWaygood on 2025-08-28 19:46_

---

_Label `ty` added by @AlexWaygood on 2025-08-28 19:46_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-28 19:46_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-28 19:46_

---

_Label `diagnostics` added by @AlexWaygood on 2025-08-28 19:46_

---

_Comment by @github-actions[bot] on 2025-08-28 19:48_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-28 19:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/server/api/artifacts.py:46:12: error[invalid-return-type] Return type does not match returned value: expected `Artifact`, found `Artifact`
+ src/prefect/server/api/artifacts.py:46:12: error[invalid-return-type] Return type does not match returned value: expected `src.prefect.server.schemas.core.Artifact`, found `src.prefect.server.database.orm_models.Artifact`

```
</details>
No memory usage changes detected ✅


---

_@carljm approved on 2025-08-28 23:07_

Thank you!

---

_Merged by @AlexWaygood on 2025-08-29 08:44_

---

_Closed by @AlexWaygood on 2025-08-29 08:44_

---

_Branch deleted on 2025-08-29 08:44_

---
