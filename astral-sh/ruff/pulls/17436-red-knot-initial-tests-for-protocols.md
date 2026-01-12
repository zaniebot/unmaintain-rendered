```yaml
number: 17436
title: "[red-knot] Initial tests for protocols"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/protocol-tests
created_at: 2025-04-16T21:13:52Z
updated_at: 2025-04-17T12:56:19Z
url: https://github.com/astral-sh/ruff/pull/17436
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Initial tests for protocols

---

_@AlexWaygood_

## Summary

There are still many tests missing from this test suite (see the long TODO list at the bottom of this file). But this PR already adds around 1,000 lines of tests, which feels like enough for now: I'd like to actually start implementing some of this stuff 😄

TODO: figure out how to make pre-commit happy... 

## Test Plan

this pr is all tests


---

_Label `red-knot` added by @AlexWaygood on 2025-04-16 21:13_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-16 21:13_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-16 21:13_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-16 21:13_

---

_Comment by @AlexWaygood on 2025-04-16 21:15_

Rendered version: https://github.com/astral-sh/ruff/blob/alex/protocol-tests/crates/red_knot_python_semantic/resources/mdtest/protocols.md

---

_Comment by @github-actions[bot] on 2025-04-16 21:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-04-16 21:16_

we really need to figure out why we aren't getting any red-knot tests run on PRs that only touch MarkDown files right now 😄

but I ran the new tests locally and I promise that they all pass!

---

_Label `testing` added by @AlexWaygood on 2025-04-16 21:32_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/protocols.md`:389 on 2025-04-17 00:39_

These should be TODOs as well

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/protocols.md`:383 on 2025-04-17 00:41_

I think there should be TODO comments here?

There are enough cases of this below that maybe it's intentional to assume any `static-assert-error` is implicitly a TODO? I don't think I like this, though; too subtle and confusing, when there are other diagnostics that are correct and not TODOs.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/protocols.md`:451 on 2025-04-17 00:41_

TODO comments?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/protocols.md`:485 on 2025-04-17 00:42_

TODO comments?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/protocols.md`:538 on 2025-04-17 00:43_

TODO comments? ...I think at this point I'll stop commenting on these, but there are more below...

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/protocols.md`:515 on 2025-04-17 00:45_

The code block below seems to be missing any demonstration of this, it only shows that they are assignable/subtype of `object`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/protocols.md`:597 on 2025-04-17 00:47_

I think it would be better to use a name here that doesn't look like it's `typing.Final`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/protocols.md`:588 on 2025-04-17 00:49_

If `Y` does provide methods or attributes that are part of `X`s interface, and the types of those are such that no subtype of `Y` could (respecting Liskov) possibly satisfy `X`s interface, we can still consider them disjoint.

---

_@carljm approved on 2025-04-17 00:53_

Fantastically thorough!

---

_@AlexWaygood reviewed on 2025-04-17 10:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/protocols.md`:383 on 2025-04-17 10:30_

Hah, yes, sorry about that. I got lazy and thought I might be able to get away without explicit TODO comments for `static-assert-error`s in this test suite, since there's no reason you'd have a `static-assert-error` in this Markdown file if it wasn't something that you wanted to pass eventually.

I'll add the TODOs :-)

---

_@AlexWaygood reviewed on 2025-04-17 10:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/protocols.md`:588 on 2025-04-17 10:39_

Indeed! This is something I wanted to discuss; thanks for bringing it up. I seem to remember us having this conversation about nominal types on one of @sharkdp's PRs (I can't easily find the reference), and IIRC you were opposed to applying the same principle there. E.g. we could consider `A` and `B` here disjoint, because they both have a covariant (read-only) `x` attribute, but the type of `A.x` is disjoint from `B.x`. There can be no possible Liskov-compliant subclass of both `A` and `B` here; they are definitely disjoint:

```py
class A:
    @property
    def x(self) -> bool:
        return True

class B:
    @property
    def x(self) -> str:
        return "x"
```

Is it justifiable to apply this principle to structural types but not nominal types? It feels inconsistent to me

---

_@AlexWaygood reviewed on 2025-04-17 10:46_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/protocols.md`:515 on 2025-04-17 10:46_

oops, thanks

---

_@AlexWaygood reviewed on 2025-04-17 11:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/protocols.md`:588 on 2025-04-17 11:35_

(Going to land the PR for now as I don't think my current language precludes the idea that we might infer other pairs of protocols as disjoint in the future. Happy to revisit it later!)

---

_Merged by @AlexWaygood on 2025-04-17 11:36_

---

_Closed by @AlexWaygood on 2025-04-17 11:36_

---

_Branch deleted on 2025-04-17 11:36_

---

_@carljm reviewed on 2025-04-17 12:56_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/protocols.md`:588 on 2025-04-17 12:56_

I do think there might be reasons to consider it for structural types and not nominal types, but I also think it's not high priority and fine to defer it either way, so let's just not worry about it for now.

---
