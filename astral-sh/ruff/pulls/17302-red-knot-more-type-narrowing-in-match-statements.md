```yaml
number: 17302
title: "[red-knot] more type-narrowing in match statements"
type: pull_request
state: merged
author: ericmarkmartin
labels:
  - ty
assignees: []
merged: true
base: main
head: more-match-narrowing
created_at: 2025-04-08T21:36:00Z
updated_at: 2025-04-18T01:18:34Z
url: https://github.com/astral-sh/ruff/pull/17302
synced_at: 2026-01-12T15:56:01Z
```

# [red-knot] more type-narrowing in match statements

---

_@ericmarkmartin_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Add more narrowing analysis for match statements:
* add narrowing constraints from guard expressions
* add negated constraints from previous predicates and guards to subsequent cases

This PR doesn't address that guards can mutate your subject, and so theoretically invalidate some of these narrowing constraints that you've previously accumulated. Some prior art on this issue [here][mutable guards].

[mutable guards]: https://www.irif.fr/~scherer/research/mutable-patterns/mutable-patterns-mlworkshop2024-abstract.pdf

## Test Plan

Add some new tests, and update some existing ones
<!-- How was it tested? -->


---

_Review requested from @carljm by @ericmarkmartin on 2025-04-08 21:36_

---

_Review requested from @AlexWaygood by @ericmarkmartin on 2025-04-08 21:36_

---

_Review requested from @sharkdp by @ericmarkmartin on 2025-04-08 21:36_

---

_Review requested from @dcreager by @ericmarkmartin on 2025-04-08 21:36_

---

_Renamed from "More match narrowing" to "[red-knot] more type-narrowing in match statements" by @ericmarkmartin on 2025-04-08 21:36_

---

_Comment by @github-actions[bot] on 2025-04-08 21:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `red-knot` added by @AlexWaygood on 2025-04-08 21:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1542 on 2025-04-09 04:16_

This comment doesn't address why we have the `|| !hash_catchall` clause above

---

_@carljm approved on 2025-04-09 04:19_

Thank you, looks good!

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-09 07:43_

---

_Comment by @carljm on 2025-04-09 15:33_

Going to go ahead and rebase this, expand the one comment slightly, and land it.

---

_Comment by @carljm on 2025-04-09 15:47_

Oops, never mind, after the rebase there's a recently-added test that fails with this PR, and it looks like the test is correct? With this PR, we seem to no longer realize that in

```py
match 42:
    case {"something": M}: ...
```

that `M` may not be bound afterward.

We should add a test for this that's not part of the star-import tests, and figure out what we need to adjust in this PR in order to avoid regressing it.

I pushed my rebase and one slightly expanded comment, but I need to move on to other things for now, so I'll consider this PR back in your court.

---

_Comment by @ericmarkmartin on 2025-04-10 05:43_

> Oops, never mind, after the rebase there's a recently-added test that fails with this PR, and it looks like the test is correct? With this PR, we seem to no longer realize that in
> 
> ```python
> match 42:
>     case {"something": M}: ...
> ```
> 
> that `M` may not be bound afterward.

it turns out it was just that I had copied the Stmt::If branch too closely: in the case of If, you must visit the test expression in the "no branch taken" flow because we need to evaluate the test before we can know it's False(y)

In the case of match statements, however, visiting the pattern implies undertaking symbol bindings, which we definitely don't want to track on the "no case matched" flow.

---

_Comment by @carljm on 2025-04-10 13:29_

Thanks for revisiting this!

Looks like we are now seeing a panic in one of the corpus tests, where we enter what should be an unreachable code path. We seem to simultaneously think there is a binding visible for a symbol, while the "unbound" initial state for that symbol is also definitely-visible -- that combination shouldn't be possible.

I took a quick glance at the code changes and nothing jumped out at me; let me know if you aren't able to figure out the reason and I can take a closer look.

---

_@carljm approved on 2025-04-18 01:13_

Looks great! Thank you for persisting with this. I pushed a couple minor simplifications I noticed, will merge once signal is green.

---

_Merged by @carljm on 2025-04-18 01:18_

---

_Closed by @carljm on 2025-04-18 01:18_

---
