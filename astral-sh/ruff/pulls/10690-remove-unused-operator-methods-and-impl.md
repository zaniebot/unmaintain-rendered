```yaml
number: 10690
title: Remove unused operator methods and impl
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/ast-cleanup
created_at: 2024-04-01T05:46:11Z
updated_at: 2024-04-02T10:23:21Z
url: https://github.com/astral-sh/ruff/pull/10690
synced_at: 2026-01-12T15:55:33Z
```

# Remove unused operator methods and impl

---

_@dhruvmanila_

## Summary

This PR removes unused operator methods and impl traits. There is already the `is_macro::Is` implementation for all the operators and this seems unnecessary.

---

_Label `internal` added by @dhruvmanila on 2024-04-01 05:46_

---

_Comment by @github-actions[bot] on 2024-04-01 06:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@AlexWaygood approved on 2024-04-01 11:26_

Makes sense to me (but I'm still relatively new to working on this code)

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:2756 on 2024-04-02 07:31_

Hum, the `ExprContext*` star types were interesting... Why did they exist when `ExprContext::Load`, `::Store` etc. exits too. Anyway, it makes sense to remove them

---

_@MichaReiser approved on 2024-04-02 07:39_

It seems we inherited all these `struct` types from `RustPython`. It's unclear to me why they existed.

---

_@dhruvmanila reviewed on 2024-04-02 10:23_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:2756 on 2024-04-02 10:23_

Yeah, I'm not sure either, maybe to convert to a Python class https://github.com/RustPython/Parser/blob/9ce55aefdeb35e2f706ce0b02d5a2dfe6295fc57/ast-pyo3/src/gen/to_py_ast.rs#L987-L997 ?

---

_Merged by @dhruvmanila on 2024-04-02 10:23_

---

_Closed by @dhruvmanila on 2024-04-02 10:23_

---

_Branch deleted on 2024-04-02 10:23_

---
