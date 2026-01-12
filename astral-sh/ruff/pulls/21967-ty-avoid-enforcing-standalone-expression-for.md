```yaml
number: 21967
title: "[ty] Avoid enforcing standalone expression for tests in f-strings"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - fuzzer
  - ty
assignees: []
merged: true
base: main
head: charlie/f
created_at: 2025-12-13T19:59:10Z
updated_at: 2025-12-16T03:31:05Z
url: https://github.com/astral-sh/ruff/pull/21967
synced_at: 2026-01-12T15:57:37Z
```

# [ty] Avoid enforcing standalone expression for tests in f-strings

---

_@charliermarsh_

## Summary

Based on what we do elsewhere and my understanding of "standalone" here...

Closes https://github.com/astral-sh/ty/issues/1865.


---

_Label `bug` added by @charliermarsh on 2025-12-13 19:59_

---

_Label `fuzzer` added by @charliermarsh on 2025-12-13 19:59_

---

_Label `ty` added by @charliermarsh on 2025-12-13 19:59_

---

_Comment by @astral-sh-bot[bot] on 2025-12-13 20:01_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-13 20:03_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2025-12-13 20:06_

---

_Review requested from @carljm by @charliermarsh on 2025-12-13 20:06_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-13 20:06_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-13 20:06_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-13 20:06_

---

_@dhruvmanila reviewed on 2025-12-15 09:38_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/infer/builder.rs`:8293 on 2025-12-15 09:38_

This also suggests that it could panic for other expressions in the f-string like (which it does):

```py
stringified_fstring_with_generator: "f'{(1 for i in range(10))}'"
stringified_fstring_with_list_comprehension: "f'{[i for i in range(10)]}'"
# similarly for dict and set comprehensions
stringified_fstring_with_boolean_expr: "f'{1 or 2}'"
```

I found this by going through the references of `infer_standalone_expression`

Can you update those as well? The comprehension ones are in two places: `infer_first_comprehension_iter` and `infer_comprehension` while the boolean expression is in `infer_boolean_expression`.

---

_@charliermarsh reviewed on 2025-12-16 02:36_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:8293 on 2025-12-16 02:36_

Done

---

_Review requested from @dhruvmanila by @charliermarsh on 2025-12-16 02:36_

---

_@dhruvmanila approved on 2025-12-16 03:05_

---

_Merged by @charliermarsh on 2025-12-16 03:31_

---

_Closed by @charliermarsh on 2025-12-16 03:31_

---

_Branch deleted on 2025-12-16 03:31_

---
