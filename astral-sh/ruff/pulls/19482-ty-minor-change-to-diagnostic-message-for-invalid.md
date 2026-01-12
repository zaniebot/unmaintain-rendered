```yaml
number: 19482
title: "[ty] Minor change to diagnostic message for invalid Literal uses"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: david/invalid-type-form-literal
created_at: 2025-07-22T09:35:07Z
updated_at: 2025-07-22T09:42:14Z
url: https://github.com/astral-sh/ruff/pull/19482
synced_at: 2026-01-12T15:56:40Z
```

# [ty] Minor change to diagnostic message for invalid Literal uses

---

_@sharkdp_

_No description provided._

---

_Label `internal` added by @sharkdp on 2025-07-22 09:35_

---

_Label `ty` added by @sharkdp on 2025-07-22 09:35_

---

_Label `diagnostics` added by @sharkdp on 2025-07-22 09:35_

---

_Review requested from @carljm by @sharkdp on 2025-07-22 09:35_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-22 09:35_

---

_Review requested from @dcreager by @sharkdp on 2025-07-22 09:35_

---

_@MichaReiser approved on 2025-07-22 09:36_

---

_@AlexWaygood approved on 2025-07-22 09:39_

---

_Comment by @github-actions[bot] on 2025-07-22 09:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
strawberry (https://github.com/strawberry-graphql/strawberry)
- error[invalid-type-form] strawberry/types/field.py:301:17: Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum value
+ error[invalid-type-form] strawberry/types/field.py:301:17: Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
- error[invalid-type-form] strawberry/types/field.py:336:17: Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum value
+ error[invalid-type-form] strawberry/types/field.py:336:17: Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member

```
</details>
No memory usage changes detected ✅


---

_Comment by @sharkdp on 2025-07-22 09:40_

> ```diff
> - error[invalid-type-form] strawberry/types/field.py:301:17: Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum value
> + error[invalid-type-form] strawberry/types/field.py:301:17: Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
> - error[invalid-type-form] strawberry/types/field.py:336:17: Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum value
> + error[invalid-type-form] strawberry/types/field.py:336:17: Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
> ```

Not that this PR has any behavioral changes, but those are true positives, by the way.

---

_Merged by @sharkdp on 2025-07-22 09:42_

---

_Closed by @sharkdp on 2025-07-22 09:42_

---

_Branch deleted on 2025-07-22 09:42_

---
