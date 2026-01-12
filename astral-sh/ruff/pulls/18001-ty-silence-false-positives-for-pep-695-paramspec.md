```yaml
number: 18001
title: "[ty] Silence false positives for PEP-695 ParamSpec annotations"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/silence-paramspec-false-positives
created_at: 2025-05-10T09:37:38Z
updated_at: 2025-05-10T09:59:27Z
url: https://github.com/astral-sh/ruff/pull/18001
synced_at: 2026-01-12T15:56:10Z
```

# [ty] Silence false positives for PEP-695 ParamSpec annotations

---

_@sharkdp_

## Summary

Suppress false positives for uses of PEP-695 `ParamSpec` in `Callable` annotations:
```py
from typing_extensions import Callable

def f[**P](c: Callable[P, int]):
    pass
```

addresses a comment here: https://github.com/astral-sh/ty/issues/157#issuecomment-2859284721

## Test Plan

Adapted Markdown tests


---

_Label `ty` added by @sharkdp on 2025-05-10 09:37_

---

_Review requested from @carljm by @sharkdp on 2025-05-10 09:37_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-10 09:37_

---

_Review requested from @dcreager by @sharkdp on 2025-05-10 09:37_

---

_Comment by @github-actions[bot] on 2025-05-10 09:41_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
- error[invalid-type-form] src/async_utils/corofunc_cache.py:28:34: The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- error[invalid-type-form] src/async_utils/corofunc_cache.py:29:34: The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- error[invalid-type-form] src/async_utils/gen_transform.py:33:17: The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- error[invalid-type-form] src/async_utils/gen_transform.py:58:33: The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- error[invalid-type-form] src/async_utils/gen_transform.py:96:17: The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- error[invalid-type-form] src/async_utils/gen_transform.py:141:17: The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- error[invalid-type-form] src/async_utils/gen_transform.py:180:17: The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- error[invalid-type-form] src/async_utils/task_cache.py:28:34: The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- error[invalid-type-form] src/async_utils/task_cache.py:29:34: The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- Found 29 diagnostics
+ Found 20 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any]`
+ error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any] | (dict[Any, Any] & list[Unknown])`

```
</details>


---

_@AlexWaygood approved on 2025-05-10 09:58_

---

_Merged by @sharkdp on 2025-05-10 09:59_

---

_Closed by @sharkdp on 2025-05-10 09:59_

---

_Branch deleted on 2025-05-10 09:59_

---
