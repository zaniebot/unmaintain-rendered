```yaml
number: 19410
title: "[ty] Shrink reachability constraints"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/shrink-reachability-constraints
created_at: 2025-07-17T19:45:49Z
updated_at: 2025-07-18T05:36:20Z
url: https://github.com/astral-sh/ruff/pull/19410
synced_at: 2026-01-12T15:56:39Z
```

# [ty] Shrink reachability constraints

---

_@MichaReiser_

## Summary

Shrink the reachability constraints before storing them in the `UseDefMap`. 
This reduces memory usage on a large project for UseDefMaps by about 400mb or around 8%

## Test Plan

`cargo test`


---

_Label `ty` added by @MichaReiser on 2025-07-17 19:45_

---

_Marked ready for review by @MichaReiser on 2025-07-17 19:49_

---

_Review requested from @carljm by @MichaReiser on 2025-07-17 19:49_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-17 19:49_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-17 19:49_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-17 19:49_

---

_Comment by @github-actions[bot] on 2025-07-17 19:49_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     memo fields = ~66MB
+     memo fields = ~63MB

trio (https://github.com/python-trio/trio)
-     memo fields = ~159MB
+     memo fields = ~152MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~332MB
+ TOTAL MEMORY USAGE: ~316MB

prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~690MB
+ TOTAL MEMORY USAGE: ~657MB

```
</details>


---

_@dcreager approved on 2025-07-17 20:46_

---

_@AlexWaygood approved on 2025-07-17 20:58_

---

_Merged by @MichaReiser on 2025-07-18 05:36_

---

_Closed by @MichaReiser on 2025-07-18 05:36_

---

_Branch deleted on 2025-07-18 05:36_

---
