```yaml
number: 21428
title: "[ty] Fix panic for cyclic star imports"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: micha/fix-exported-names-too-many-iterations
created_at: 2025-11-13T13:46:36Z
updated_at: 2025-11-13T14:38:11Z
url: https://github.com/astral-sh/ruff/pull/21428
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Fix panic for cyclic star imports

---

_Pull request opened by @MichaReiser on 2025-11-13 13:46_

## Summary

Fixes https://github.com/astral-sh/ty/issues/444

This PR fixes a panic where the `export_names` query never converged for a cyclic star import. 

This is a rather silly bug. The cycle never converged because the symbol ordering changed between iterations (forever).... 


Here's a diff between the exported symbols before the fix for the last two iterations

https://www.diffchecker.com/tk3ihAdo/


This PR fixes the bug by simply sorting the names before returning.

I considered using a `BTreeMap` but found the sort slightly simpler (and both roughly have the same complexity)

## Test Plan

I recreated the MRE from the linked issue and verified that it panics without my changes.


---

_Label `bug` added by @MichaReiser on 2025-11-13 13:46_

---

_Label `ty` added by @MichaReiser on 2025-11-13 13:46_

---

_Review requested from @carljm by @MichaReiser on 2025-11-13 13:46_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-11-13 13:46_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-13 13:46_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-13 13:46_

---

_Comment by @astral-sh-bot[bot] on 2025-11-13 13:49_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-13 13:49_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 45 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @AlexWaygood on 2025-11-13 14:00_

Oh, wow, and the iteration order matters because the query itself returns `Box<[Name]>` rather than returning the `FxHashMap` directly (because if it returned the `FxHashMap` directly, two maps with the same entries would still compare equal even if they were differently ordered)... what a bug.....

---

_@AlexWaygood approved on 2025-11-13 14:04_

brilliant

---

_Comment by @MichaReiser on 2025-11-13 14:04_

Come on Github... start those CI jobs. I want to see the perf impact. 

---

_Merged by @MichaReiser on 2025-11-13 14:38_

---

_Closed by @MichaReiser on 2025-11-13 14:38_

---

_Branch deleted on 2025-11-13 14:38_

---
