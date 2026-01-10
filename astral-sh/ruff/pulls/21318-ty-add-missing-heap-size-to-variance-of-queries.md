```yaml
number: 21318
title: "[ty] Add missing `heap_size` to `variance_of` queries"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/fix-heap-size
created_at: 2025-11-07T16:13:23Z
updated_at: 2025-11-07T16:18:29Z
url: https://github.com/astral-sh/ruff/pull/21318
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Add missing `heap_size` to `variance_of` queries

---

_Pull request opened by @MichaReiser on 2025-11-07 16:13_

_No description provided._

---

_Review requested from @carljm by @MichaReiser on 2025-11-07 16:13_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-11-07 16:13_

---

_Label `internal` added by @MichaReiser on 2025-11-07 16:13_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-07 16:13_

---

_Label `ty` added by @MichaReiser on 2025-11-07 16:13_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-07 16:13_

---

_Comment by @github-actions[bot] on 2025-11-07 16:15_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-07 16:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`

sphinx (https://github.com/sphinx-doc/sphinx)
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`

prefect (https://github.com/PrefectHQ/prefect)
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`

```
</details>


---

_@AlexWaygood approved on 2025-11-07 16:17_

---

_Merged by @MichaReiser on 2025-11-07 16:18_

---

_Closed by @MichaReiser on 2025-11-07 16:18_

---

_Branch deleted on 2025-11-07 16:18_

---
