```yaml
number: 15994
title: "[red-knot] Fix diagnostic range for non-iterable unpacking assignments"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: david/unpacking-diagnostic-range
created_at: 2025-02-06T14:25:56Z
updated_at: 2025-02-06T14:36:24Z
url: https://github.com/astral-sh/ruff/pull/15994
synced_at: 2026-01-12T15:55:53Z
```

# [red-knot] Fix diagnostic range for non-iterable unpacking assignments

---

_@sharkdp_

## Summary

I noticed that the diagnostic range in specific unpacking assignments is wrong. For this example

```py
a, b = 1
```

we previously got (see first commit):

```
error: lint:not-iterable
 --> /src/mdtest_snippet.py:1:1
  |
1 | a, b = 1
  | ^^^^ Object of type `Literal[1]` is not iterable
  |
```

and with this change, we get:

```
error: lint:not-iterable
 --> /src/mdtest_snippet.py:1:8
  |
1 | a, b = 1
  |        ^ Object of type `Literal[1]` is not iterable
  |
```

## Test Plan

New snapshot tests.

---

_Label `red-knot` added by @sharkdp on 2025-02-06 14:25_

---

_Label `diagnostics` added by @sharkdp on 2025-02-06 14:25_

---

_Review requested from @carljm by @sharkdp on 2025-02-06 14:25_

---

_Review requested from @MichaReiser by @sharkdp on 2025-02-06 14:25_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-02-06 14:25_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unpacking.md_-_Unpacking_-_Too_few_values_to_unpack.snap`:25 on 2025-02-06 14:29_

This is probably a case where we'd want multiple diagnostic ranges (@BurntSushi) but having the assignment target being the main range seems correct.

---

_@MichaReiser approved on 2025-02-06 14:29_

---

_@sharkdp reviewed on 2025-02-06 14:33_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unpacking.md_-_Unpacking_-_Too_few_values_to_unpack.snap`:25 on 2025-02-06 14:33_

Yes, I just included these here to make sure that we're not generally moving the diagnostic range to the right hand side of the assignment. Just for this very specific case I fixed (where `unwrap_diagnostic_range` really expects the expression of the iterable, not the assignment target).

---

_@dhruvmanila approved on 2025-02-06 14:35_

Thank you!

---

_Merged by @sharkdp on 2025-02-06 14:36_

---

_Closed by @sharkdp on 2025-02-06 14:36_

---

_Branch deleted on 2025-02-06 14:36_

---
