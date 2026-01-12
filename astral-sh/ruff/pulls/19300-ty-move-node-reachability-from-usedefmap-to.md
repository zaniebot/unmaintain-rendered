```yaml
number: 19300
title: "[ty] Move `node_reachability` from `UseDefMap` to `SemanticIndex`"
type: pull_request
state: closed
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
base: main
head: micha/node-reachability-index
created_at: 2025-07-12T17:02:41Z
updated_at: 2025-10-13T17:51:18Z
url: https://github.com/astral-sh/ruff/pull/19300
synced_at: 2026-01-12T15:56:36Z
```

# [ty] Move `node_reachability` from `UseDefMap` to `SemanticIndex`

---

_@MichaReiser_

## Summary

ty builds `UseDefMaps` by scope because it allows Salsa to short-circuit cache validation for queries that 
only depend on a specific `use_def_map` (e.g. from function a) if the `use_def_map` for that scope hasn't changed. 
For this to work, it's important that a single `UseDefMap` only stores data that remains stable if its corresponding scope hasn't changed. This is the case for all the `Scoped*Id`s, but not for `NodeKey`.
Inserting a new expression at the top of the file has the result that every single `NodeKey` changes, invalidating all `UseDefMap`s. 

This PR moves the `node_reachability` out of the scope specific `UseDefMap` and into the `SemanticIndex` (which is for the entire file)
to allow Salsa to backdate `use_def_map` queries more often.

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2025-07-12 17:02_

---

_Label `ty` added by @MichaReiser on 2025-07-12 17:02_

---

_Comment by @github-actions[bot] on 2025-07-12 17:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     memo fields = ~66MB
+     memo fields = ~63MB

prefect (https://github.com/PrefectHQ/prefect)
-     memo fields = ~568MB
+     memo fields = ~541MB

```
</details>


---

_Marked ready for review by @MichaReiser on 2025-07-13 18:51_

---

_Review requested from @carljm by @MichaReiser on 2025-07-13 18:51_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-13 18:51_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-13 18:51_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-13 18:51_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-13 21:14_

---

_@sharkdp approved on 2025-07-14 06:58_

Thank you Micha!

I assume it's not that easy to write a regression test for this? It might still be worth it, given that this went unnoticed for quite some time? I assume it was also not easy to find the root cause here?

---

_Closed by @MichaReiser on 2025-10-13 17:51_

---
