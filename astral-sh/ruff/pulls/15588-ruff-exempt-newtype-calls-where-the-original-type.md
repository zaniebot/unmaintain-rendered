```yaml
number: 15588
title: "[`ruff`] Exempt `NewType` calls where the original type is immutable (`RUF009`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
assignees: []
merged: true
base: main
head: RUF009
created_at: 2025-01-20T00:27:40Z
updated_at: 2025-01-20T14:51:12Z
url: https://github.com/astral-sh/ruff/pull/15588
synced_at: 2026-01-10T20:05:43Z
```

# [`ruff`] Exempt `NewType` calls where the original type is immutable (`RUF009`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-20 00:27_

## Summary

Resolves #6447.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-20 00:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/function_call_in_dataclass_default.rs`:190 on 2025-01-20 05:24_

nit: should we inline this function in `is_immutable_newtype_call` ?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/function_call_in_dataclass_default.rs`:204 on 2025-01-20 05:25_

Isn't the following also a valid form (using keyword arguments) ?
```py
from typing import NewType

UserId = NewType(name="UserId", tp=int)
```

---

_@dhruvmanila reviewed on 2025-01-20 05:25_

---

_Label `bug` added by @dhruvmanila on 2025-01-20 05:26_

---

_@InSyncWithFoo reviewed on 2025-01-20 05:32_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/function_call_in_dataclass_default.rs`:204 on 2025-01-20 05:32_

I have never seen `NewType` being used with keyword arguments in the wild. Not to mention, Mypy [requires](https://mypy-play.net/?mypy=1.14.1&python=3.13&flags=strict&gist=6952eb32be908326df80a5872a205a87) that `NewType` be called with two positional arguments. Changing the logic is easy enough, but is it worth it?

---

_@InSyncWithFoo reviewed on 2025-01-20 05:35_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/function_call_in_dataclass_default.rs`:190 on 2025-01-20 05:35_

Maybe not. The two are comparably long.

---

_@dhruvmanila reviewed on 2025-01-20 05:48_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/function_call_in_dataclass_default.rs`:204 on 2025-01-20 05:48_

That's a good point.

I don't see any mention of whether `NewType` is required to be used with positional arguments either on the Python documentation nor in any of the PEPs. I think we should support them as I see usages of that on GitHub: https://github.com/search?q=NewType(name%3D+language:Python&type=code

---

_@MichaReiser reviewed on 2025-01-20 09:50_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/typing.rs`:991 on 2025-01-20 09:50_

Is this used anywhere?

---

_@InSyncWithFoo reviewed on 2025-01-20 13:23_

---

_Review comment by @InSyncWithFoo on `crates/ruff_python_semantic/src/analyze/typing.rs`:991 on 2025-01-20 13:23_

If I recall correctly, it was added for completeness. Every other `TypeChecker` has a corresponding `is_*` function.

---

_@MichaReiser reviewed on 2025-01-20 13:24_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/typing.rs`:991 on 2025-01-20 13:24_

I prefer only adding code that is used somewhere. 

---

_Review comment by @dhruvmanila on `crates/ruff_python_semantic/src/analyze/typing.rs`:886 on 2025-01-20 14:37_

I think this is unused now?

```suggestion
```

---

_@dhruvmanila approved on 2025-01-20 14:37_

---

_@dhruvmanila reviewed on 2025-01-20 14:39_

---

_Review comment by @dhruvmanila on `crates/ruff_python_semantic/src/analyze/typing.rs`:991 on 2025-01-20 14:39_

Somehow I missed looking at this file while reviewing 🤦 

---

_Merged by @dhruvmanila on 2025-01-20 14:44_

---

_Closed by @dhruvmanila on 2025-01-20 14:44_

---

_Branch deleted on 2025-01-20 14:51_

---
