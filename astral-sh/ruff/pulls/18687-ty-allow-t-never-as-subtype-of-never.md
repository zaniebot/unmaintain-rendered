```yaml
number: 18687
title: "[ty] allow `T: Never` as subtype of `Never`"
type: pull_request
state: merged
author: felixscherz
labels:
  - ty
assignees: []
merged: true
base: main
head: fix/never-as-subtype-of-type-variables
created_at: 2025-06-15T18:43:18Z
updated_at: 2025-06-16T17:46:17Z
url: https://github.com/astral-sh/ruff/pull/18687
synced_at: 2026-01-12T15:56:23Z
```

# [ty] allow `T: Never` as subtype of `Never`

---

_@felixscherz_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

Hi, this is intended to resolve https://github.com/astral-sh/ty/issues/638.

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Check `Type::has_relation_to` for the upper bound of a `TypeVarInstance` when checking the relation to `Never`.

## Test Plan

Added markdown tests.


---

_Review requested from @carljm by @felixscherz on 2025-06-15 18:43_

---

_Review requested from @AlexWaygood by @felixscherz on 2025-06-15 18:43_

---

_Review requested from @sharkdp by @felixscherz on 2025-06-15 18:43_

---

_Review requested from @dcreager by @felixscherz on 2025-06-15 18:43_

---

_Comment by @github-actions[bot] on 2025-06-15 18:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `ty` added by @AlexWaygood on 2025-06-15 20:29_

---

_@AlexWaygood approved on 2025-06-16 17:43_

thanks!

---

_Merged by @AlexWaygood on 2025-06-16 17:46_

---

_Closed by @AlexWaygood on 2025-06-16 17:46_

---
