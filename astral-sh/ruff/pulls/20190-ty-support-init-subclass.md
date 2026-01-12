```yaml
number: 20190
title: "[ty] Support `__init_subclass__`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-1106
created_at: 2025-09-01T07:24:15Z
updated_at: 2025-09-01T08:16:30Z
url: https://github.com/astral-sh/ruff/pull/20190
synced_at: 2026-01-12T15:56:56Z
```

# [ty] Support `__init_subclass__`

---

_@sharkdp_

## Summary

`__init_subclass__` is implicitly a classmethod.

closes https://github.com/astral-sh/ty/issues/1106

## Test Plan

Regression test

---

_Review requested from @carljm by @sharkdp on 2025-09-01 07:24_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-01 07:24_

---

_Review requested from @dcreager by @sharkdp on 2025-09-01 07:24_

---

_Label `ty` added by @sharkdp on 2025-09-01 07:24_

---

_Comment by @github-actions[bot] on 2025-09-01 07:27_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-01 07:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review requested from @MichaReiser by @sharkdp on 2025-09-01 07:38_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/function.rs`:729 on 2025-09-01 08:03_

Should `__new__` be included as well given that it's an implicit class method?

---

_@dhruvmanila approved on 2025-09-01 08:03_

---

_@sharkdp reviewed on 2025-09-01 08:10_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/function.rs`:729 on 2025-09-01 08:10_

`__new__` takes `cls` as its first argument, but it's a static method, not a classmethod (https://docs.python.org/3/reference/datamodel.html#object.__new__). `cls` needs to be passed explicitly.

---

_@sharkdp reviewed on 2025-09-01 08:12_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/function.rs`:729 on 2025-09-01 08:12_

But `__class_getitem__` could probably go here? Will check separately.

---

_Merged by @sharkdp on 2025-09-01 08:16_

---

_Closed by @sharkdp on 2025-09-01 08:16_

---

_Branch deleted on 2025-09-01 08:16_

---
