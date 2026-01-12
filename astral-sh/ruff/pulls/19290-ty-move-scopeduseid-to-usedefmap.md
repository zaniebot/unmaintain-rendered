```yaml
number: 19290
title: "[ty] Move `ScopedUseId` to `UseDefMap`"
type: pull_request
state: closed
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
draft: true
base: main
head: micha/delete-ast-ids
created_at: 2025-07-11T18:30:47Z
updated_at: 2025-07-13T15:13:29Z
url: https://github.com/astral-sh/ruff/pull/19290
synced_at: 2026-01-12T15:56:36Z
```

# [ty] Move `ScopedUseId` to `UseDefMap`

---

_@MichaReiser_

## Summary

Remove `AstIds` and store the `node` -> `ScopedUseId` on the `UseDefMap` instead.

The `UseDefMap` already stores node sensitive data with `node_reachability`.
That means, the use def map is changing for every change in a file already anyway and doesn't benefit
from the indirection that `AstIds` introduces. We can, therefore, store `uses_by_node` directly on the `UseDefMap`. 

If we plan on improving the incrementality in the future, then this is possible by splitting the `UseDefMap` into 
two structs:

* A `UseDefMapSourceMap` that stores `uses_by_node` and `node_reachability`. 
* The `UseDefMap...` that stores all other fields. This one should be behind its own salsa query to support backdating.

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2025-07-11 18:30_

---

_Label `ty` added by @MichaReiser on 2025-07-11 18:30_

---

_Label `internal` added by @MichaReiser on 2025-07-11 18:30_

---

_Label `ty` added by @MichaReiser on 2025-07-11 18:30_

---

_Comment by @github-actions[bot] on 2025-07-11 18:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
- TOTAL MEMORY USAGE: ~76MB
+ TOTAL MEMORY USAGE: ~80MB

trio (https://github.com/python-trio/trio)
-     memo fields = ~152MB
+     memo fields = ~159MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     memo fields = ~260MB
+     memo fields = ~273MB

```
</details>


---

_Closed by @MichaReiser on 2025-07-13 15:13_

---
