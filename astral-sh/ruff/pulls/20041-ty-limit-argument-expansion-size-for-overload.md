```yaml
number: 20041
title: "[ty] Limit argument expansion size for overload call evaluation"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/limit-argument-expansion-size
created_at: 2025-08-22T09:26:12Z
updated_at: 2025-08-25T09:43:18Z
url: https://github.com/astral-sh/ruff/pull/20041
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Limit argument expansion size for overload call evaluation

---

_Pull request opened by @dhruvmanila on 2025-08-22 09:26_

## Summary

This PR limits the argument type expansion size for an overload call evaluation to 512.

The limit chosen is arbitrary but I've taken the 256 limit from Pyright into account and bumped it x2 to start with.

Initially, I actually started out by trying to refactor the entire argument type expansion to be lazy. Currently, expanding a single argument at any position eagerly creates the combination (argument lists) and returns that (`Vec<CallArguments>`) but I thought we could make it lazier by converting the return type of `expand` from `Iterator<Item = Vec<CallArguments>>` to `Iterator<Item = Iterator<Item = CallArguments>>` but that's proving to be difficult to implement mainly because we **need** to maintain the previous expansion to generate the next expansion which is the main reason to use `std::iter::successors` in the first place.

Another approach would be to eagerly expand all the argument types and then use the `combinations` from `itertools` to generate the combinations but we would need to find the "boundary" between arguments lists produced from expanding argument at position 1 and position 2 because that's important for the algorithm.

Closes: https://github.com/astral-sh/ty/issues/868

## Test Plan

Add test case to demonstrate the limit along with the diagnostic snapshot stating that the limit has been reached.


---

_Label `ty` added by @dhruvmanila on 2025-08-22 09:26_

---

_Comment by @github-actions[bot] on 2025-08-22 09:28_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-22 09:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser reviewed on 2025-08-22 09:31_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/call/arguments.rs`:181 on 2025-08-22 09:31_

Sorry for commenting on a draft PR. Is there some information that we could include to make it easier for users to narrow down where this is happening (e.g. the full signature of the called function?, or some information about the call site?)

---

_@dhruvmanila reviewed on 2025-08-22 09:34_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/arguments.rs`:181 on 2025-08-22 09:34_

I think we could expand the diagnostic of `no-matching-overload` to include this information as a note for users to notice this.

---

_@dhruvmanila reviewed on 2025-08-22 13:55_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/snapshots/overloads.md_-_Overloads_-_Argument_type_expans…_-_Optimization___Limit_…_(cd61048adbc17331).snap`:121 on 2025-08-22 13:55_

I'm not exactly sure how best to convey this to the user in a way that they can understand. It seems that this kind of information would require additional context, possibly even linking to the overload call evaluation section of the typing spec. If anyone has any suggestions, I'd appreciate that.

---

_Marked ready for review by @dhruvmanila on 2025-08-22 13:55_

---

_Review requested from @carljm by @dhruvmanila on 2025-08-22 13:55_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-08-22 13:55_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-08-22 13:55_

---

_Review requested from @dcreager by @dhruvmanila on 2025-08-22 13:55_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/overloads.md`:864 on 2025-08-22 18:31_

A couple nits: 

1. `ab` parameter seems unused here
2. I think it's pretty implicit / unclear that we infer `Unknown | None` for `a` due to lack of annotation, plus default type -- but this is crucial to the test, because that's the union we repeatedly try to expand. Why not explicitly annotate `a: int | None` and leave out the default?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/snapshots/overloads.md_-_Overloads_-_Argument_type_expans…_-_Optimization___Limit_…_(cd61048adbc17331).snap`:121 on 2025-08-22 18:35_

I think the piece of info that might be most useful to the user would be which argument types we were trying to expand. Not sure how hard that would be to add.

I also think this limit should ideally be high enough that almost no user should ever see it in realistic code, so we maybe don't need to spend much time perfecting the diagnostic; the key thing is not to have runaway execution time / memory.

---

_@carljm approved on 2025-08-22 18:36_

---

_Comment by @carljm on 2025-08-22 18:40_

> proving to be difficult to implement mainly because we **need** to maintain the previous expansion to generate the next expansion

I think it is good to move on from this issue now, if the limit takes care of the runaway problem on the fuzzer code sample, and we don't have reports of this in real code. But for future, I'm curious -- why is the above hard to implement? It sounds like a custom-implemented iterator that stores some state, which doesn't seem particularly unusual?

---

_@dhruvmanila reviewed on 2025-08-25 09:14_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/snapshots/overloads.md_-_Overloads_-_Argument_type_expans…_-_Optimization___Limit_…_(cd61048adbc17331).snap`:121 on 2025-08-25 09:14_

> I think the piece of info that might be most useful to the user would be which argument types we were trying to expand. Not sure how hard that would be to add.

This isn't too difficult i.e., I can add the argument index value at which point the expansion exceeded the maximum limit.

> I also think this limit should ideally be high enough that almost no user should ever see it in realistic code, so we maybe don't need to spend much time perfecting the diagnostic; the key thing is not to have runaway execution time / memory.

Yes, agreed.

---

_@dhruvmanila reviewed on 2025-08-25 09:17_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/call/overloads.md`:864 on 2025-08-25 09:17_

Thanks! I think that makes sense. I'll also update the parameter type for other functions to have an explicit annotation and not depend on the implicit behavior of having the default value without an annotation.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-25 09:19_

---

_Comment by @dhruvmanila on 2025-08-25 09:31_

> But for future, I'm curious -- why is the above hard to implement? It sounds like a custom-implemented iterator that stores some state, which doesn't seem particularly unusual?

I'll try to explain it using this example: `(bool, int | str, A | B | C)`, assuming that we're at the final argument in the expansion process, one of the data that we'd need to maintain is all the argument lists that resulted in expanding all the previous arguments i.e., 1 and 2 in this example. This means that when the expansion is at `A | B | C` argument, the data would contain `(True, int, ...), (False, int, ...), (True, str, ...), (False, str, ...)` (4) argument lists. Imagine this at a larger scale where the previous iteration could generate 200 argument lists which needs to be kept between iteration.

One way to resolve this could be that we only maintain the expanded types and generate the combinations for each iteration manually i.e., in the above example, for the first argument, we'd generate 2 combinations by expanding `bool` to `True` and `False` and we'll store the expanded list. Then, for the second argument, we'll expand `int | str` and generate 4 combinations. In this implementation, we'd save memory but each iteration would have to generate the combinations every time.

Does this make sense?

But, now that I think this out loud, maybe the second solution isn't too bad.

---

_Merged by @dhruvmanila on 2025-08-25 09:43_

---

_Closed by @dhruvmanila on 2025-08-25 09:43_

---

_Branch deleted on 2025-08-25 09:43_

---
