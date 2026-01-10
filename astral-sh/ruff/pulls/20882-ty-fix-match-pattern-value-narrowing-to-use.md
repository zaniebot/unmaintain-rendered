```yaml
number: 20882
title: "[ty] Fix match pattern value narrowing to use equality semantics"
type: pull_request
state: merged
author: ericmarkmartin
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: fix-match-value-narrowing
created_at: 2025-10-15T04:50:34Z
updated_at: 2025-10-18T01:03:55Z
url: https://github.com/astral-sh/ruff/pull/20882
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Fix match pattern value narrowing to use equality semantics

---

_Pull request opened by @ericmarkmartin on 2025-10-15 04:50_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves https://github.com/astral-sh/ty/issues/1349.

Fix match statement value patterns to use equality comparison semantics instead of incorrectly narrowing to literal types directly. Value patterns use equality for matching, and equality can be overridden, so we can't always narrow to the matched literal.

## Test Plan

Updated match.md with corrected expected types and an additional example with explanation


---

_Review requested from @carljm by @ericmarkmartin on 2025-10-15 04:50_

---

_Review requested from @AlexWaygood by @ericmarkmartin on 2025-10-15 04:50_

---

_Review requested from @sharkdp by @ericmarkmartin on 2025-10-15 04:50_

---

_Review requested from @dcreager by @ericmarkmartin on 2025-10-15 04:50_

---

_Comment by @github-actions[bot] on 2025-10-15 04:52_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-15 04:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `bug` added by @AlexWaygood on 2025-10-15 07:43_

---

_Label `ty` added by @AlexWaygood on 2025-10-15 07:43_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/match.md`:137 on 2025-10-15 11:16_

I think we should update these tests here to see that `x` in the guard expression *can* be a narrowed type.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/match.md`:163 on 2025-10-15 11:18_

Similar here. Can we maybe update the type of `x` (to a large union) to restore the spirit of this test.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/match.md`:186 on 2025-10-15 11:18_

Same here.

---

_@sharkdp approved on 2025-10-15 11:20_

Thank you very much. It would be great if we could update some of the old tests in order to restore the original intention. Otherwise, this looks great.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-10-15 14:23_

---

_@sharkdp approved on 2025-10-16 07:46_

Thank you for the update

---

_Merged by @sharkdp on 2025-10-16 07:50_

---

_Closed by @sharkdp on 2025-10-16 07:50_

---

_Branch deleted on 2025-10-18 01:03_

---
