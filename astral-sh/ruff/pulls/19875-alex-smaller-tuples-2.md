```yaml
number: 19875
title: alex/smaller tuples 2
type: pull_request
state: closed
author: AlexWaygood
labels:
  - do-not-merge
  - ty
assignees: []
draft: true
base: main
head: alex/smaller-tuples-2
created_at: 2025-08-12T10:58:36Z
updated_at: 2025-08-12T11:11:39Z
url: https://github.com/astral-sh/ruff/pull/19875
synced_at: 2026-01-10T17:52:17Z
```

# alex/smaller tuples 2

---

_Pull request opened by @AlexWaygood on 2025-08-12 10:58_

- **[ty] Reduce memory usage of `TupleSpec` and `TupleType`**
- **cherry-pick Ibraheem's changes on top**

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Label `do-not-merge` added by @AlexWaygood on 2025-08-12 10:58_

---

_Label `ty` added by @AlexWaygood on 2025-08-12 10:58_

---

_Closed by @AlexWaygood on 2025-08-12 10:59_

---

_Branch deleted on 2025-08-12 10:59_

---

_Comment by @github-actions[bot] on 2025-08-12 11:01_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-12 11:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`

trio (https://github.com/python-trio/trio)
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
-     struct fields = ~10MB
+     struct fields = ~11MB

sphinx (https://github.com/sphinx-doc/sphinx)
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
-     struct fields = ~15MB
+     struct fields = ~16MB

prefect (https://github.com/PrefectHQ/prefect)
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
+ WARN expected `heap_size` to be provided by Salsa struct `BoundTypeVarInstance`
-     struct fields = ~33MB
+     struct fields = ~40MB

```
</details>


---

_Comment by @github-actions[bot] on 2025-08-12 11:11_

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
