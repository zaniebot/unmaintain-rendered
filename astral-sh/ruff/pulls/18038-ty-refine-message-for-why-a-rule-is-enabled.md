```yaml
number: 18038
title: "[ty] Refine message for why a rule is enabled"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: micha/refine-rule-level-hint
created_at: 2025-05-12T11:22:14Z
updated_at: 2025-05-13T14:18:45Z
url: https://github.com/astral-sh/ruff/pull/18038
synced_at: 2026-01-12T15:56:10Z
```

# [ty] Refine message for why a rule is enabled

---

_@MichaReiser_

## Summary

I find `rule xx is enabled by default` reads more natural than when omitting the word *rule*, now that the rule code is no longer prefixed with `lint:`

## Test Plan

See snapshots


---

_Review requested from @carljm by @MichaReiser on 2025-05-12 11:22_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-12 11:22_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-12 11:22_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-12 11:22_

---

_Label `ty` added by @MichaReiser on 2025-05-12 11:22_

---

_Comment by @github-actions[bot] on 2025-05-12 11:25_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any] | (dict[Any, Any] & list[Unknown])`
+ error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any]`

```
</details>


---

_@sharkdp approved on 2025-05-12 11:30_

---

_Merged by @MichaReiser on 2025-05-12 11:31_

---

_Closed by @MichaReiser on 2025-05-12 11:31_

---

_Branch deleted on 2025-05-12 11:31_

---

_Label `diagnostics` added by @dhruvmanila on 2025-05-13 14:18_

---
