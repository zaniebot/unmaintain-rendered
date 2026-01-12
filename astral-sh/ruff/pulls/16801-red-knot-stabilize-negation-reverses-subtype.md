```yaml
number: 16801
title: "[red-knot] Stabilize `negation_reverses_subtype_order` property test"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-intersection-ordering
created_at: 2025-03-17T12:24:01Z
updated_at: 2025-03-17T12:33:41Z
url: https://github.com/astral-sh/ruff/pull/16801
synced_at: 2026-01-12T15:55:58Z
```

# [red-knot] Stabilize `negation_reverses_subtype_order` property test

---

_@AlexWaygood_

## Summary

This is a re-creation of https://github.com/astral-sh/ruff/pull/16764 by @mtshiba, which I closed meaning to immediately reopen (GitHub wasn't updating the PR with the latest pushed changes), and which GitHub will not allow me to reopen for some reason. Pasting the summary from that PR below:

> From https://github.com/astral-sh/ruff/pull/16641
> 
> As stated in this comment (https://github.com/astral-sh/ruff/pull/16641#discussion_r1996153702), the current ordering implementation for intersection types is incorrect. So, I will introduce lexicographic ordering for intersection types.

## Test Plan

One property test stabilised (tested locally with `QUICKCHECK_TESTS=2000000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::stable::negation_reverses_subtype_order`), and existing mdtests that previously failed now pass.

Primarily-authored-by: [mtshiba](https://github.com/astral-sh/ruff/commits?author=mtshiba)


---

_Label `red-knot` added by @AlexWaygood on 2025-03-17 12:24_

---

_Review requested from @carljm by @AlexWaygood on 2025-03-17 12:24_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-03-17 12:24_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-03-17 12:24_

---

_Comment by @github-actions[bot] on 2025-03-17 12:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @AlexWaygood on 2025-03-17 12:33_

---

_Closed by @AlexWaygood on 2025-03-17 12:33_

---

_Branch deleted on 2025-03-17 12:33_

---
