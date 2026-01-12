```yaml
number: 17408
title: "[red-knot] Add regression tests for narrowing constraints cycles"
type: pull_request
state: merged
author: cake-monotone
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: cake/regression-test-for-narrowing-query-panic
created_at: 2025-04-15T11:48:46Z
updated_at: 2025-04-15T14:27:55Z
url: https://github.com/astral-sh/ruff/pull/17408
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Add regression tests for narrowing constraints cycles

---

_@cake-monotone_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

closes #17215 

This PR adds regression tests for the following cycled queries:
- all_narrowing_constraints_for_expression
- all_negative_narrowing_constraints_for_expression

The following test files are included:
- `red_knot_project/resources/test/corpus/cycle_narrowing_constraints.py`
- `red_knot_project/resources/test/corpus/cycle_negative_narrowing_constraints.py`

These test names don't follow the existing naming convention based on Cinder.
However, I’ve chosen these names to clearly reflect the regression cases.
Let me know if you’d prefer to align more closely with the existing Cinder-based style.

## Test Plan

```sh
git checkout 1a6a10b30
cargo test --package red_knot_project  -- corpus
```


---

_Review requested from @carljm by @cake-monotone on 2025-04-15 11:48_

---

_Review requested from @MichaReiser by @cake-monotone on 2025-04-15 11:48_

---

_Review requested from @AlexWaygood by @cake-monotone on 2025-04-15 11:48_

---

_Review requested from @sharkdp by @cake-monotone on 2025-04-15 11:48_

---

_Review requested from @dcreager by @cake-monotone on 2025-04-15 11:48_

---

_Label `testing` added by @AlexWaygood on 2025-04-15 11:49_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-15 11:49_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-15 11:50_

---

_Comment by @github-actions[bot] on 2025-04-15 11:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @cake-monotone on 2025-04-15 11:53_

Commit 1a6a10b30 is the commit right before #17209.

---

_@carljm approved on 2025-04-15 14:27_

Thank you!!

---

_Merged by @carljm on 2025-04-15 14:27_

---

_Closed by @carljm on 2025-04-15 14:27_

---
