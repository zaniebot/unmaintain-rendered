```yaml
number: 18613
title: "[`pylint`] De-emphasize `__hash__ = Parent.__hash__` (`PLW1641`)"
type: pull_request
state: merged
author: ntBre
labels:
  - documentation
assignees: []
merged: true
base: main
head: brent/plw1641-docs
created_at: 2025-06-10T18:04:24Z
updated_at: 2025-06-10T18:25:17Z
url: https://github.com/astral-sh/ruff/pull/18613
synced_at: 2026-01-12T15:56:22Z
```

# [`pylint`] De-emphasize `__hash__ = Parent.__hash__` (`PLW1641`)

---

_@ntBre_

Summary
--

This PR updates the docs for PLW1641 to place less emphasis on the example of inheriting a parent class's `__hash__` implementation by both reducing the length of the example and warning that it may be unsound in general, as @AlexWaygood pointed out on Notion.

Test plan
--

Existing tests

---

_Review requested from @AlexWaygood by @ntBre on 2025-06-10 18:04_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/eq_without_hash.rs`:19 on 2025-06-10 18:07_

```suggestion
/// method implicitly set to `None`, regardless of if a superclass defines
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/eq_without_hash.rs`:22 on 2025-06-10 18:08_

```suggestion
/// cause issues when using instances of the class as keys in a dictionary or
/// members of a set.
```

---

_@AlexWaygood approved on 2025-06-10 18:08_

Perfect, thank you! Some unrelated wordsmithing that we could also do while we're here:

---

_Label `documentation` added by @AlexWaygood on 2025-06-10 18:08_

---

_Comment by @github-actions[bot] on 2025-06-10 18:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @ntBre on 2025-06-10 18:21_

---

_Closed by @ntBre on 2025-06-10 18:21_

---

_Branch deleted on 2025-06-10 18:21_

---
