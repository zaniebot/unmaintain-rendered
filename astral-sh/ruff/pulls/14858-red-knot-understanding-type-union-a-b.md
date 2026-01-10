```yaml
number: 14858
title: "[red-knot] Understanding `type[Union[A, B]]`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-type-union
created_at: 2024-12-09T05:13:56Z
updated_at: 2024-12-09T12:52:52Z
url: https://github.com/astral-sh/ruff/pull/14858
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Understanding `type[Union[A, B]]`

---

_Pull request opened by @InSyncWithFoo on 2024-12-09 05:13_

## Summary

Resolves #14834.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2024-12-09 05:13_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2024-12-09 05:13_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2024-12-09 05:13_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2024-12-09 05:13_

---

_Comment by @InSyncWithFoo on 2024-12-09 05:51_

The failing test boils down to this:

```py
import collections

MyObj1 = collections.namedtuple("MyObj1", ["a", "b"])
```



---

_Label `red-knot` added by @AlexWaygood on 2024-12-09 07:46_

---

_@MichaReiser had review dismissed on 2024-12-09 08:19_

The tests are failing with a panic in an assertion. Would you mind taking a look at what's happening

---

_@AlexWaygood approved on 2024-12-09 12:36_

Thanks. There were a couple of issues with your initial patch; I've pushed to your branch to fix them:
- You didn't handle single-element unions such as `type[Union[int]]` (the subscript slice inside the `Union[]` is not an `Expr::Tuple` if there's only a single element).
- You were failing to store the types of subexpressions for subscripts that are either invalid or not-yet-supported; that was causing panics in the corpus test. It can be surprisingly tricky to make sure that you infer the type of each subexpression exactly once; it took me longer than I expected to fix this!
- Going forward, we'd prefer to use parameter annotations in mdtests to test type expressions rather than return annotations, as they're more concise (https://github.com/astral-sh/ruff/issues/14839)

---

_Comment by @github-actions[bot] on 2024-12-09 12:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @AlexWaygood on 2024-12-09 12:47_

---

_Closed by @AlexWaygood on 2024-12-09 12:47_

---

_Branch deleted on 2024-12-09 12:49_

---

_Comment by @InSyncWithFoo on 2024-12-09 12:51_

@AlexWaygood Thanks!

---
