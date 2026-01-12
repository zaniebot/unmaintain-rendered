```yaml
number: 16856
title: "[red-knot] add test cases result in false positive errors"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: false-positive-tests
created_at: 2025-03-19T18:19:59Z
updated_at: 2025-03-20T17:19:38Z
url: https://github.com/astral-sh/ruff/pull/16856
synced_at: 2026-01-12T15:55:59Z
```

# [red-knot] add test cases result in false positive errors

---

_@mtshiba_

## Summary

From #16641

The previous PR attempted to fix the errors presented in this PR, but as discussed in the conversation, it was concluded that the approach was undesirable and that further work would be needed to fix the errors with a correct general solution.

In this PR, I instead add the test cases from the previous PR as TODOs, as a starting point for future work.

## Test Plan



---

_Review requested from @carljm by @mtshiba on 2025-03-19 18:20_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-03-19 18:20_

---

_Review requested from @sharkdp by @mtshiba on 2025-03-19 18:20_

---

_Review requested from @dcreager by @mtshiba on 2025-03-19 18:20_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-19 18:21_

---

_Comment by @github-actions[bot] on 2025-03-19 18:25_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-03-19 19:38_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:700 on 2025-03-20 17:09_

These two seem higher priority to follow up on, because we don't just fail to simplify the intersection, we simplify it to a plainly incorrect type.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_gradual_equivalent_to.md`:54 on 2025-03-20 17:11_

I think the dynamic type reduction from your previous PR, which would fix these cases, is pretty solid (not overly special-cased) and could be put up as a separate PR?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:316 on 2025-03-20 17:13_

These are duplicated from above. It seems they belong above (under "Union types" heading), not here under "Intersection types."
```suggestion
```

---

_@carljm approved on 2025-03-20 17:13_

Looks good, thank you!

---

_Merged by @carljm on 2025-03-20 17:17_

---

_Closed by @carljm on 2025-03-20 17:17_

---
