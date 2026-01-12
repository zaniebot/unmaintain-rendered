```yaml
number: 17461
title: "[red-knot] allow assignment expression in call compare narrowing"
type: pull_request
state: merged
author: MatthewMckee4
labels: []
assignees: []
merged: true
base: main
head: cover-more-assignment-expressions
created_at: 2025-04-18T14:43:26Z
updated_at: 2025-04-18T15:47:23Z
url: https://github.com/astral-sh/ruff/pull/17461
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] allow assignment expression in call compare narrowing

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

There was some narrowing constraints not covered from the previous PR

```py
def _(x: object):
    if (type(y := x)) is bool:
        reveal_type(y)  # revealed: bool
```

Also, refactored a bit

## Test Plan

Update type_api.md 


---

_Review requested from @carljm by @MatthewMckee4 on 2025-04-18 14:43_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-04-18 14:43_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-04-18 14:43_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-04-18 14:43_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/narrow.rs`:251 on 2025-04-18 14:43_

Im not sure the best place to put a function like this

---

_@MatthewMckee4 reviewed on 2025-04-18 14:43_

---

_Comment by @github-actions[bot] on 2025-04-18 14:45_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:251 on 2025-04-18 15:43_

I think this is a fine spot for now!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:512 on 2025-04-18 15:44_

Nice!

---

_@carljm approved on 2025-04-18 15:46_

Great catch, and nice implementation!!

---

_Merged by @carljm on 2025-04-18 15:46_

---

_Closed by @carljm on 2025-04-18 15:46_

---

_Branch deleted on 2025-04-18 15:47_

---
