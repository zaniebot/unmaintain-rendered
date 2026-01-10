```yaml
number: 16899
title: "[red-knot] Avoid false-positive diagnostics on `*` import statements"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/star-import-no-error
created_at: 2025-03-21T14:34:24Z
updated_at: 2025-03-21T14:41:50Z
url: https://github.com/astral-sh/ruff/pull/16899
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] Avoid false-positive diagnostics on `*` import statements

---

_Pull request opened by @AlexWaygood on 2025-03-21 14:34_

## Summary

This PR removes false-positive diagnostics for `*` imports. Currently we always emit a diagnostic for these statements unless the module we're importing from has a symbol named `"*"` in its symbol table for the global scope. (And if we were doing everything correctly, no module ever would have a symbol named `"*"` in its global scope!)

The fix here is sort-of hacky and won't be what we'll want to do long-term. However, I think it's useful to do this as a first step since:
- It significantly reduces false positives when running on code that uses `*` imports
- It "resets" the tests to a cleaner state with many fewer TODOs, making it easier to see what the hard work is that's still to be done.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2025-03-21 14:34_

---

_Review requested from @carljm by @AlexWaygood on 2025-03-21 14:34_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-03-21 14:34_

---

_Review requested from @dcreager by @AlexWaygood on 2025-03-21 14:34_

---

_Comment by @github-actions[bot] on 2025-03-21 14:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @AlexWaygood on 2025-03-21 14:41_

---

_Closed by @AlexWaygood on 2025-03-21 14:41_

---

_Branch deleted on 2025-03-21 14:41_

---
