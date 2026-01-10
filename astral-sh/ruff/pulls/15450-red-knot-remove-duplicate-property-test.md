```yaml
number: 15450
title: "[red-knot] Remove duplicate property test"
type: pull_request
state: merged
author: cake-monotone
labels:
  - ty
assignees: []
merged: true
base: main
head: red-knot-remove-same-property-tests
created_at: 2025-01-13T02:53:24Z
updated_at: 2025-01-13T10:45:49Z
url: https://github.com/astral-sh/ruff/pull/15450
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Remove duplicate property test

---

_Pull request opened by @cake-monotone on 2025-01-13 02:53_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Follow-up PR from https://github.com/astral-sh/ruff/pull/15415  🥲 

The exact same property test already exists: `intersection_assignable_to_both` and `all_type_pairs_can_be_assigned_from_their_intersection`

## Test Plan

`cargo test -p red_knot_python_semantic -- --ignored types::property_tests::flaky`



---

_Review requested from @carljm by @cake-monotone on 2025-01-13 02:53_

---

_Review requested from @MichaReiser by @cake-monotone on 2025-01-13 02:53_

---

_Review requested from @AlexWaygood by @cake-monotone on 2025-01-13 02:53_

---

_Review requested from @sharkdp by @cake-monotone on 2025-01-13 02:53_

---

_Comment by @github-actions[bot] on 2025-01-13 02:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @dhruvmanila on 2025-01-13 06:07_

---

_@sharkdp approved on 2025-01-13 07:18_

Thanks.

---

_Merged by @sharkdp on 2025-01-13 07:18_

---

_Closed by @sharkdp on 2025-01-13 07:18_

---

_Branch deleted on 2025-01-13 10:45_

---
