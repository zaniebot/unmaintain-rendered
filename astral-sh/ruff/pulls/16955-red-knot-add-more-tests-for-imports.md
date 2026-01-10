```yaml
number: 16955
title: "[red-knot] Add more tests for `*` imports"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/more-star-import-tests
created_at: 2025-03-24T16:29:12Z
updated_at: 2025-03-24T16:41:07Z
url: https://github.com/astral-sh/ruff/pull/16955
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] Add more tests for `*` imports

---

_Pull request opened by @AlexWaygood on 2025-03-24 16:29_

## Summary

This PR separates out the entirely new tests from https://github.com/astral-sh/ruff/pull/16923 into a standalone PR. I'll rebase https://github.com/astral-sh/ruff/pull/16923 on top of this branch.

The reasons for separating it out are:
- It should make it clearer to see in <https://github.com/astral-sh/ruff/pull/16923> exactly how the functionality is changing (we can see the assertions in the tests _change_, which isn't so obvious if the tests are entirely new)
- The diff on <https://github.com/astral-sh/ruff/pull/16923> is getting pretty big; this should reduce the diff on that PR somewhat
- These tests seem useful in and of themselves, so even if we need to do a wholesale revert of <https://github.com/astral-sh/ruff/pull/16923> for whatever reason, it'll be nice to keep the tests

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `testing` added by @AlexWaygood on 2025-03-24 16:29_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-24 16:29_

---

_Review requested from @carljm by @AlexWaygood on 2025-03-24 16:29_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-03-24 16:29_

---

_Review requested from @dcreager by @AlexWaygood on 2025-03-24 16:29_

---

_@carljm approved on 2025-03-24 16:32_

---

_Comment by @github-actions[bot] on 2025-03-24 16:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @AlexWaygood on 2025-03-24 16:39_

---

_Closed by @AlexWaygood on 2025-03-24 16:39_

---

_Branch deleted on 2025-03-24 16:39_

---
