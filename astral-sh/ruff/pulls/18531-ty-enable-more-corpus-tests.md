```yaml
number: 18531
title: "[ty] Enable more corpus tests"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/more-corpus-tests
created_at: 2025-06-07T12:48:52Z
updated_at: 2025-06-07T13:18:27Z
url: https://github.com/astral-sh/ruff/pull/18531
synced_at: 2026-01-12T15:56:20Z
```

# [ty] Enable more corpus tests

---

_@AlexWaygood_

## Summary

We now no longer panic on any of the files covered by the `linter_stubs_no_panic()` test, so we can safely enable that without adding any new files to the `KNOWN_FAILURES` list 🎉

We still do panic on several files in typeshed when attempting to pull types, but the number is now low enough that we can just list the failing files in `KNOWN_FAILURES`. Somewhat concerning is that we appear to have stack overflows when attempting to pull types in any typeshed files where the last segement of the path is "types.pyi". We should look into that -- but I still think it's worth expanding the corpus test to run over most typeshed files for now, to prevent future regressions.

## Test Plan

`cargo test -p ty_project --test check`

---

_Review requested from @carljm by @AlexWaygood on 2025-06-07 12:48_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-06-07 12:48_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-07 12:48_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-07 12:48_

---

_Label `testing` added by @AlexWaygood on 2025-06-07 12:48_

---

_Label `ty` added by @AlexWaygood on 2025-06-07 12:48_

---

_Comment by @github-actions[bot] on 2025-06-07 12:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser approved on 2025-06-07 13:02_

---

_Merged by @AlexWaygood on 2025-06-07 13:18_

---

_Closed by @AlexWaygood on 2025-06-07 13:18_

---

_Branch deleted on 2025-06-07 13:18_

---
