```yaml
number: 17605
title: "[red-knot] fix collapsing literal and its negation to object"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/unionfix
created_at: 2025-04-24T12:36:13Z
updated_at: 2025-04-24T13:56:08Z
url: https://github.com/astral-sh/ruff/pull/17605
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] fix collapsing literal and its negation to object

---

_Pull request opened by @carljm on 2025-04-24 12:36_

## Summary

Another follow-up to the unions-of-large-literals optimization. Restore the behavior that e.g. `Literal[""] | ~Literal[""]` collapses to `object`.

## Test Plan

Added mdtests.


---

_Label `red-knot` added by @carljm on 2025-04-24 12:36_

---

_Review requested from @AlexWaygood by @carljm on 2025-04-24 12:36_

---

_Review requested from @sharkdp by @carljm on 2025-04-24 12:36_

---

_Review requested from @dcreager by @carljm on 2025-04-24 12:36_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/union.md`:185 on 2025-04-24 12:39_

nit: is there a reason why we're using the pre-PEP-604 syntax here...? I find it more readable if we can keep the bracketese to a minimum:

```suggestion
    a: Literal["a", ""] | Not[AlwaysFalsy],
    b: Literal["a", ""] | Not[Literal[""]],
    c: Literal[""] | Not[Literal[""]],
    d: Not[Literal[""]] | Literal[""],
```

---

_Comment by @github-actions[bot] on 2025-04-24 12:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/union.md`:1 on 2025-04-24 12:44_

It seems like this bug also affected the bytes-literal branches and the int-literal branches, but I don't see any tests for those, even though they're different code paths. Could we add this test too?

```py
def f(
    a: Literal["a"] | Not[Literal["a"]],
    b: Literal[b"b"] | Not[Literal[b"b"]],
    c: Literal[42] | Not[Literal[42],
):
    reveal_type(a)  # revealed: object
    reveal_type(b)  # revealed: object
    reveal_type(c)  # revealed: object
```

---

_@AlexWaygood approved on 2025-04-24 12:45_

This looks reasonable. Though I'm _slightly_ concerned about the spiralling complexity here.

It would be nice if we could find a way to unify the branches here so that we don't have to repeat the same somewhat complex logic three times over. But that doesn't have to be done in this PR; it can wait for a followup.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/union.md`:1 on 2025-04-24 13:50_

Also added the reverse direction of each, because they are handled in different code paths.

---

_@carljm reviewed on 2025-04-24 13:50_

---

_@carljm reviewed on 2025-04-24 13:50_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/union.md`:185 on 2025-04-24 13:50_

Once you start bracketing, it's just hard to stop.

---

_Merged by @carljm on 2025-04-24 13:55_

---

_Closed by @carljm on 2025-04-24 13:55_

---

_Branch deleted on 2025-04-24 13:55_

---
