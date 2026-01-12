```yaml
number: 17419
title: "[red-knot] set a size limit on unions of literals"
type: pull_request
state: merged
author: carljm
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: cjm/unionlimit2
created_at: 2025-04-16T02:56:15Z
updated_at: 2025-04-16T15:39:34Z
url: https://github.com/astral-sh/ruff/pull/17419
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] set a size limit on unions of literals

---

_@carljm_

## Summary

Until we optimize our full union/intersection representation to efficiently handle large numbers of same-kind literal types "as a block", set a fairly low limit on the size of unions of literals.

We will want to increase this limit once we've made the broader efficiency improvement (tracked in https://github.com/astral-sh/ty/issues/124).

## Test Plan

`cargo bench --bench red_knot`


---

_Label `red-knot` added by @carljm on 2025-04-16 02:56_

---

_Review requested from @AlexWaygood by @carljm on 2025-04-16 02:56_

---

_Review requested from @sharkdp by @carljm on 2025-04-16 02:56_

---

_Review requested from @dcreager by @carljm on 2025-04-16 02:56_

---

_Comment by @github-actions[bot] on 2025-04-16 02:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-04-16 03:02_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Funionlimit2)

### Merging astral-sh/ruff#17419 will **improve performances by ×110**

<sub>Comparing <code>cjm/unionlimit2</code> (ac691ca) with <code>main</code> (5a115e7)</sub>



### Summary

`⚡ 1` improvements  
`✅ 32` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` red_knot_micro[many_string_assignments] `` | 5,816.3 ms | 51.9 ms | ×110 |


---

_Label `performance` added by @AlexWaygood on 2025-04-16 07:40_

---

_@sharkdp reviewed on 2025-04-16 07:48_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:172 on 2025-04-16 07:48_

Returning early here seems potentially problematic, because we might miss some simplifications that would trigger below if we were to "properly" (remove the literals and then) add `int` to the union.

For example, if `bool` is already part of the union, and we go beyond the 200 int literals limit, we could end up with `bool | int`, violating the *"No type in a union can be a subtype of any other type in the union"* invariant documented above:

```py
from typing import Literal

def _(literals_2: Literal[0, 1], b: bool, flag: bool):
    literals_4 = 2 * literals_2 + literals_2     # Literal[0, 1, 2, 3]
    literals_16 = 4 * literals_4 + literals_4    # Literal[0, 1, .., 15]
    literals_64 = 4 * literals_16 + literals_4   # Literal[0, 1, .., 63]
    literals_128 = 2 * literals_64 + literals_2  # Literal[0, 1, .., 127]

    # Going beyond the MAX_UNION_LITERALS limit:
    literals_256 = 16 * literals_16 + literals_16
    reveal_type(literals_256)  # revealed: int

    # Going beyond the limit when another type is already part of the union
    bool_and_literals_128 = b if flag else literals_128  # bool | Literal[0, 1, ..., 127]
    literals_128_shifted = literals_128 + 128  # Literal[128, 129, ..., 255]

    # Now union the two:
    reveal_type(bool_and_literals_128 if flag else literals_128_shifted)  # revealed: bool | int
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:172 on 2025-04-16 14:03_

Ah, of course, thank you!

---

_@carljm reviewed on 2025-04-16 14:03_

---

_@carljm reviewed on 2025-04-16 14:17_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:172 on 2025-04-16 14:17_

And thank you for writing the test for me :)

---

_Merged by @carljm on 2025-04-16 14:23_

---

_Closed by @carljm on 2025-04-16 14:23_

---

_Branch deleted on 2025-04-16 14:23_

---

_@sharkdp reviewed on 2025-04-16 14:25_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:107 on 2025-04-16 14:25_

Couldn't we return here directly and get rid of the `too_large` flags?
```suggestion
                                let replace_with = KnownClass::Str.to_instance(self.db);
                                return self.add(replace_with);
```

---

_@carljm reviewed on 2025-04-16 15:39_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:107 on 2025-04-16 15:39_

Yes indeed, thank you. I had tried this initially (but without the `replace_with` variable) and had borrow checker issues on `self`, which I initially mistakenly thought had to with the iteration over `&mut self.elements`, so I moved it out of the loop. Then I realized the borrow checker actually had a problem with accessing `self.db` inside a call to `self.add` (which takes ownership of `self`) and fixed that, but failed to realize I could move it back inside the loop.

https://github.com/astral-sh/ruff/pull/17429

---
