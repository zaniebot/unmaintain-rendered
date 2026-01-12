```yaml
number: 18072
title: "Use `https://ty.dev/rules` when linking to the rules table"
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: micha/rules-url
created_at: 2025-05-13T16:39:42Z
updated_at: 2025-05-13T17:21:08Z
url: https://github.com/astral-sh/ruff/pull/18072
synced_at: 2026-01-12T15:56:11Z
```

# Use `https://ty.dev/rules` when linking to the rules table

---

_@MichaReiser_

## Summary

This makes it easier to update the location of the markdown file in the future




---

_Review requested from @carljm by @MichaReiser on 2025-05-13 16:39_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-13 16:39_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-13 16:39_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-13 16:39_

---

_Label `documentation` added by @MichaReiser on 2025-05-13 16:39_

---

_Label `ty` added by @MichaReiser on 2025-05-13 16:39_

---

_@dhruvmanila approved on 2025-05-13 16:41_

---

_Comment by @dhruvmanila on 2025-05-13 16:42_

Maybe `ty.dev/reference/rules`? But, I think that could change once we have docs website so let's leave it for now.

---

_Comment by @github-actions[bot] on 2025-05-13 16:43_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any]`
+ error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any] | (dict[Any, Any] & list[Unknown])`
+ error[type-assertion-failure] tests/annotations/declarations.py:951:5: Argument does not have asserted type `FullBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/annotations/declarations.py:956:5: Argument does not have asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/annotations/declarations.py:961:5: Argument does not have asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- Found 647 diagnostics
+ Found 650 diagnostics

```
</details>


---

_@carljm approved on 2025-05-13 17:18_

---

_Merged by @MichaReiser on 2025-05-13 17:21_

---

_Closed by @MichaReiser on 2025-05-13 17:21_

---

_Branch deleted on 2025-05-13 17:21_

---
