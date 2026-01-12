```yaml
number: 21297
title: "[ty] Make range/position conversions fallible"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/lsp-fallible-range-conversion
created_at: 2025-11-06T15:45:18Z
updated_at: 2025-11-07T14:44:25Z
url: https://github.com/astral-sh/ruff/pull/21297
synced_at: 2026-01-12T15:57:20Z
```

# [ty] Make range/position conversions fallible

---

_@MichaReiser_

## Summary

This PR refactors `to_lsp_range`, `to_lsp_position`, and `to_text_size`, and `to_text_range` return an `Option`
because the operations can fail if the document is a notebook and the notebook isn't open in the editor.

## Test Plan

`cargo test`. I clicked around in VS Code


---

_Label `internal` added by @MichaReiser on 2025-11-06 15:45_

---

_Label `ty` added by @MichaReiser on 2025-11-06 15:45_

---

_Label `internal` added by @MichaReiser on 2025-11-06 15:45_

---

_Label `ty` added by @MichaReiser on 2025-11-06 15:45_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-11-06 15:46_

---

_Comment by @github-actions[bot] on 2025-11-06 15:47_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Marked ready for review by @MichaReiser on 2025-11-06 15:47_

---

_Review requested from @carljm by @MichaReiser on 2025-11-06 15:47_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-06 15:47_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-06 15:47_

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-06 15:47_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-06 15:47_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-11-06 15:47_

---

_Comment by @github-actions[bot] on 2025-11-06 15:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`

sphinx (https://github.com/sphinx-doc/sphinx)
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`

prefect (https://github.com/PrefectHQ/prefect)
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

```
</details>


---

_Review comment by @Gankra on `crates/ty_server/src/document/range.rs`:73 on 2025-11-06 20:56_

```suggestion
    /// Returns the uri of the text document this position belongs to.
```

---

_@Gankra approved on 2025-11-06 20:59_

---

_Comment by @Gankra on 2025-11-06 21:00_

Curse you LSP protocol notebook APIs!!!

---

_Merged by @MichaReiser on 2025-11-07 14:44_

---

_Closed by @MichaReiser on 2025-11-07 14:44_

---

_Branch deleted on 2025-11-07 14:44_

---
