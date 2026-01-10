```yaml
number: 21106
title: "[ty] Rename `inner` query for better debugging experience"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/rename-query
created_at: 2025-10-28T11:21:17Z
updated_at: 2025-10-28T12:46:04Z
url: https://github.com/astral-sh/ruff/pull/21106
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Rename `inner` query for better debugging experience

---

_Pull request opened by @MichaReiser on 2025-10-28 11:21_

## Summary

Using an unspecific name like `inner` makes it difficult to find the problematic query if Salsa panics because the cycle head iterated too many times, or if any other invariant isn't true. 

This specific instance came up on https://github.com/astral-sh/ruff/pull/20566 where the Salsa panic only referred to `inner`.


This PR renames the `inner` function to use a longer name so that the query's debug name is more self explanatory ;)

## Test Plan

The panic message now renders as `Cycle recovery function for infer_definition_types(Id(a42f)) introduced a cycle, depending on is_equivalent_to_object_inner(Id(8007)). This is not allowed.` which
is more actionable.


---

_Label `internal` added by @MichaReiser on 2025-10-28 11:21_

---

_Review requested from @carljm by @MichaReiser on 2025-10-28 11:21_

---

_Label `ty` added by @MichaReiser on 2025-10-28 11:21_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-10-28 11:21_

---

_Review requested from @sharkdp by @MichaReiser on 2025-10-28 11:21_

---

_Review requested from @dcreager by @MichaReiser on 2025-10-28 11:21_

---

_Comment by @github-actions[bot] on 2025-10-28 11:23_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-28 11:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @MichaReiser on 2025-10-28 11:26_

---

_Closed by @MichaReiser on 2025-10-28 11:26_

---

_Branch deleted on 2025-10-28 11:26_

---

_@dcreager reviewed on 2025-10-28 12:46_

👍 

---
