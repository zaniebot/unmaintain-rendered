```yaml
number: 20359
title: "[ty] no more diverging query cycles in type expressions"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/divergent-generic
created_at: 2025-09-12T01:41:49Z
updated_at: 2025-09-16T23:44:12Z
url: https://github.com/astral-sh/ruff/pull/20359
synced_at: 2026-01-10T17:40:28Z
```

# [ty] no more diverging query cycles in type expressions

---

_Pull request opened by @carljm on 2025-09-12 01:41_

## Summary

Use `Type::Divergent` to short-circuit diverging types in type expressions. This avoids panicking in a wide variety of cases of recursive type expressions.

Avoids many panics (but not yet all -- I'll be tracking down the rest) from https://github.com/astral-sh/ty/issues/256 by falling back to Divergent. For many of these recursive type aliases, we'd like to support them properly (i.e. really understand the recursive nature of the type, not just fall back to Divergent) but that will be future work.

This switches `Type::has_divergent_type` from using `any_over_type` to a custom set of visit methods, because `any_over_type` visits more than we need to visit, and exercises some lazy attributes of type, causing significantly more work. This change means this diff doesn't regress perf; it even reclaims some of the perf regression from https://github.com/astral-sh/ruff/pull/20333.

## Test Plan

Added mdtest for recursive type alias that panics on main.

Verified that we can now type-check `packaging` (and projects depending on it) without panic; this will allow moving a number of mypy-primer projects from `bad.txt` to `good.txt` in a subsequent PR.


---

_Label `ty` added by @carljm on 2025-09-12 01:41_

---

_Comment by @github-actions[bot] on 2025-09-12 01:45_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-12 01:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-09-12 01:56_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fdivergent-generic?runnerMode=WallTime)

### Merging #20359 will **improve performances by 5.89%**

<sub>Comparing <code>cjm/divergent-generic</code> (814c42d) with <code>main</code> (c3f2187)</sub>



### Summary

`⚡ 1` improvement  
`✅ 7` untouched  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` small[tanjun] `` | 1.7 s | 1.6 s | +5.89% |


---

_Marked ready for review by @carljm on 2025-09-12 02:01_

---

_Review requested from @AlexWaygood by @carljm on 2025-09-12 02:01_

---

_Review requested from @sharkdp by @carljm on 2025-09-12 02:01_

---

_Review requested from @dcreager by @carljm on 2025-09-12 02:01_

---

_Comment by @carljm on 2025-09-12 02:05_

@mtshiba I can't request you as reviewer, but would be happy for you to take a look at this PR.

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer.rs`:122 on 2025-09-12 04:39_

This approach (sets an upper limit on cycles and falls back to inference to set the cycle initial value to `Divergent` instead of `Never` when it is detected that the cycle is "not likely to converge") seems to have already been adopted in #20333, but I think it isn't the best.
For example, suppose a divergent function has `n+m` branches for its return values, of which `m(>1)` are divergent. After the 10th cycle, the size of the return type will be `n(m^10 - 1)/(m-1)` (10th value of `size(n) = m size(n-1) + n; size(1) = n`).
Although cases where `m` is large are likely to be rare, if they do occur, it will become a bottleneck.

```
1st: T1 | T2 | ... | Tn (| list[Never] | tuple[Never])
2nd: T1 | T2 | ... | | Tn | list[T1 | T2 | ... | Tn] | tuple[T1 | T2 | ... | Tn]
3rd: T1 | T2 | ... | Tn | list[T1 | T2 | ... | Tn | list[T1 | T2 | ... | Tn] | tuple[T1 | T2 | ... | Tn]] | tuple[T1 | T2 | ... | Tn | list[T1 | T2 | ... | Tn] | tuple[T1 | T2 | ... | Tn]]
...
```

In fact, while working on #17371, I also felt it would be nice for salsa to do multi-cycle convergence checks even in fallbacks, but finally decided it wasn't necessary in this case.

I think that in this kind of inference, the initial cycle value should be set to the `Divergent` type from the beginning.
`Divergent` has the identity property with respect to union, like `Never`. So `Divergent` will not appear in the result type if the recursive inference is not actually divergent.

```
int | Divergent = int | (int | (int | ...)) = int
```

This means that we should add special rules for `Divergent` to the union type reduction rules. They have already been discussed [here](https://github.com/astral-sh/ruff/pull/17371#issuecomment-3279706332).
I plan to split #17371 again so we can use the rules here.

---

_@mtshiba reviewed on 2025-09-12 04:41_

---

_Comment by @AlexWaygood on 2025-09-12 12:44_

> For many of these recursive type aliases, we'd like to support them properly (i.e. really understand the recursive nature of the type, not just fall back to Divergent) but that will be future work.

Will it still be easy to identify type aliases in user code that we don't properly support, if they no longer cause us to panic? The panics are obviously really bad UX, but the nice thing about them is it makes it very easy for us to spot where we have bugs

---

_Comment by @carljm on 2025-09-12 16:50_

> > For many of these recursive type aliases, we'd like to support them properly (i.e. really understand the recursive nature of the type, not just fall back to Divergent) but that will be future work.
> 
> Will it still be easy to identify type aliases in user code that we don't properly support, if they no longer cause us to panic? The panics are obviously really bad UX, but the nice thing about them is it makes it very easy for us to spot where we have bugs

I agree this is a potential downside, but a) I think it's outweighed by the value of eliminating the cycles ASAP, and b) I'm not that worried about it. We already know the core recursive-alias cases that we need to support, and if there are a few edge-case bugs hiding, if they matter to anyone we'll discover them.

It's also quite easy to temporarily remove the `Type::Divergent` fallback, if we want to do that and run it on ecosystem projects as a bug-finding tool.

---

_Comment by @AlexWaygood on 2025-09-12 16:56_

Sounds good!

---

_@carljm reviewed on 2025-09-12 17:02_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:122 on 2025-09-12 17:02_

I agree that the arbitrary choice of 10 cycles before falling back is not ideal. I think in practice it will be unlikely to be a problem, though.

I think the idea that we could make `Divergent` behave sufficiently similarly to `Never` that we could use it as the initial iteration type is interesting, but I'm not confident it will be able to handle all cases correctly.

For the moment I plan to continue pushing forward on addressing existing panics with this approach, but I'll be very interested to take a look if you propose a PR that can improve this. 

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:27 on 2025-09-15 09:23_

Should we maybe add a reveal-type assertion here to "get notified" when this changes?
```suggestion
Recursive = list[Union["Recursive", None]]

def _(r: Recursive):
    reveal_type(r)  # revealed: list[Divergent]
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:38 on 2025-09-15 09:24_

Similar here?

```suggestion
Recursive = list["Recursive" | None]

def f(r: Recursive):
    reveal_type(r)  # revealed: list[Divergent]
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:6555 on 2025-09-15 09:25_

It's not immediately clear to me why descending into e.g. typevar bounds/constraints is not necessary. Could the reason be explained in this comment?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:6545 on 2025-09-15 09:38_

Have you thought about implementing `has_divergent_type` using a custom `TypeVisitor` (similar to what `any_over_type` does), that would overwrite selected methods like `visit_bound_type_var_type` in order to skip visiting the parts that this needs/wants to skip?

---

_@sharkdp approved on 2025-09-15 09:39_

Thank you very much!

Very much looking forward to fewer panics related to these.

---

_Comment by @marcelroed on 2025-09-15 19:53_

Here's an unhandled cycle using the latest commit:

```python
from typing import Generic, TypeVar

AT = TypeVar("AT", bound="A")
BT = TypeVar("BT", bound="B")

class A(Generic[BT]):
    ...

class B(Generic[AT]):
    ...
```

This is from an ML modeling library where type A (model configs) need to be able to construct type B (model instances) and type B (model instances) need to be able to return type A (model configs). Feel free to delete/hide this comment if it should be a part of a separate issue instead.

---

_Comment by @carljm on 2025-09-15 19:58_

@marcelroed Yes, thanks -- I'm aware this PR doesn't fix all the cases in https://github.com/astral-sh/ty/issues/256. Your case is already mentioned in https://github.com/astral-sh/ty/issues/256#issuecomment-3140776680, so it will be addressed before we close that issue.

---

_@carljm reviewed on 2025-09-16 02:28_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6555 on 2025-09-16 02:28_

It's because they are lazy/deferred (not part of the core identity of the type), which means they won't cause a query cycle in type inference, and we don't want to trigger their inference unnecessarily.

Currently this is actually only true for PEP 695 type vars, but it should become true for all typevars; that's on my roadmap.

Added a comment about this.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6545 on 2025-09-16 23:31_

Thanks for the prompt. I tried this but was slightly unhappy with how many types the visitor needed to acquire detailed knowledge of the internals of in order to correctly implement visiting types without visiting lazily-inferred type attributes. So I ended up creating a general visitor flag for this, so that each type's `walk_` function can implement this distinction internally.

---

_@carljm reviewed on 2025-09-16 23:31_

---

_Merged by @carljm on 2025-09-16 23:44_

---

_Closed by @carljm on 2025-09-16 23:44_

---

_Branch deleted on 2025-09-16 23:44_

---
