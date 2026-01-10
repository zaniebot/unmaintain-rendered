```yaml
number: 17447
title: "[red-knot] Some pairs of types are no longer recognised as assignable to their union"
type: issue
state: closed
author: github-actions
labels:
  - bug
  - ty
assignees: []
created_at: 2025-04-17T12:13:16Z
updated_at: 2025-04-18T15:20:04Z
url: https://github.com/astral-sh/ruff/issues/17447
synced_at: 2026-01-10T11:09:58Z
```

# [red-knot] Some pairs of types are no longer recognised as assignable to their union

---

_Issue opened by @github-actions on 2025-04-17 12:13_

Failing property test run here: https://github.com/astral-sh/ruff/actions/runs/14515316525

---

_Label `bug` added by @github-actions[bot] on 2025-04-17 12:13_

---

_Label `red-knot` added by @github-actions[bot] on 2025-04-17 12:13_

---

_Label `testing` added by @github-actions[bot] on 2025-04-17 12:13_

---

_Comment by @AlexWaygood on 2025-04-17 12:17_

Bisects to a1f361949eaee3571b0c0777c1bbcd09d4e1b190 -- cc. @carljm 

```
a1f361949eaee3571b0c0777c1bbcd09d4e1b190 is the first bad commit
commit a1f361949eaee3571b0c0777c1bbcd09d4e1b190
Author: Carl Meyer <carl@astral.sh>
Date:   Wed Apr 16 06:55:37 2025 -0700

    [red-knot] optimize building large unions of literals (#17403)
    
    ## Summary
    
    Special-case literal types in `UnionBuilder` to speed up building large
    unions of literals.
    
    This optimization is extremely effective at speeding up building even a
    very large union (it improves the large-unions benchmark by 41x!). The
    problem we can run into is that it is easy to then run into another
    operation on the very large union (for instance, narrowing may add it to
    an intersection, which then distributes it over the intersection) which
    is still slow.
    
    I think it is possible to avoid this by extending this optimized
    "grouped" representation throughout not just `UnionBuilder`, but all of
    our union and intersection representations. I have some work in this
    direction, but rather than spending more time on it right now, I'd
    rather just land this much, along with a limit on the size of these
    unions (to avoid building really big unions quickly and then hitting
    issues where they are used.)
    
    ## Test Plan
    
    Existing tests and benchmarks.
    
    ---------
    
    Co-authored-by: Alex Waygood <Alex.Waygood@Gmail.com>

 .../resources/mdtest/annotations/literal.md        |   4 +-
 .../resources/mdtest/annotations/string.md         |   2 +-
 .../red_knot_python_semantic/src/types/builder.rs  | 166 ++++++++++++++++++---
 3 files changed, 149 insertions(+), 23 deletions(-)
```

---

_Comment by @AlexWaygood on 2025-04-17 12:23_

The failing test in CI is:

```
 failures:

---- types::property_tests::stable::all_type_pairs_are_assignable_to_their_union stdout ----

thread 'types::property_tests::stable::all_type_pairs_are_assignable_to_their_union' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Union([StringLiteral("a"), StringLiteral("")]), AlwaysTruthy)
```

Locally, I'm also seeing failures in `all_fully_static_type_pairs_are_subtype_of_their_union`, such as:

```
---- types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union stdout ----

thread 'types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union' panicked at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Union([BytesLiteral(""), BytesLiteral("\0")]), AlwaysFalsy)
```

---

_Renamed from "Daily property test run failed on Thu Apr 17 2025" to "[red-knot] Some pairs of types are no longer recognised as assignable to their union" by @AlexWaygood on 2025-04-17 12:24_

---

_Label `testing` removed by @AlexWaygood on 2025-04-17 12:25_

---

_Assigned to @carljm by @carljm on 2025-04-17 12:56_

---

_Comment by @carljm on 2025-04-17 12:56_

Yeah I understand the issue and I'm on it, thanks!

---

_Closed by @carljm on 2025-04-18 15:20_

---
