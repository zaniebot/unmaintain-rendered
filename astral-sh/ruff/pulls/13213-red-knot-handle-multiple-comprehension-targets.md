```yaml
number: 13213
title: "[red-knot] Handle multiple comprehension targets"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/comprehension-multiple-target
created_at: 2024-09-02T12:41:13Z
updated_at: 2024-09-04T14:20:50Z
url: https://github.com/astral-sh/ruff/pull/13213
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] Handle multiple comprehension targets

---

_@dhruvmanila_

## Summary

Part of #13085, this PR updates the comprehension definition to handle multiple targets.

## Test Plan

Update existing semantic index test case for comprehension with multiple targets. Running corpus tests shouldn't panic.


---

_Label `red-knot` added by @dhruvmanila on 2024-09-02 12:41_

---

_Renamed from "[red-knot] Handle multiple for comprehension targets" to "[red-knot] Handle multiple comprehension targets" by @dhruvmanila on 2024-09-02 12:41_

---

_Marked ready for review by @dhruvmanila on 2024-09-02 12:48_

---

_Review requested from @carljm by @dhruvmanila on 2024-09-02 12:48_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-09-02 12:48_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-09-02 12:48_

---

_Comment by @github-actions[bot] on 2024-09-02 12:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review request for @AlexWaygood removed by @AlexWaygood on 2024-09-02 13:41_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:159 on 2024-09-03 18:42_

I was wondering if we'll also need access to the `if` portion of the generator here, but I think we won't; that expression should be handled separately as a `Constraint`, it's not part of the `Definition`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1656 on 2024-09-03 19:15_

We've inherited some awfully confusing naming from the CPython grammar and AST :( If we have `[y for x in [] for y in x]`, that's a single "list comprehension", whose AST node has a `generators` attribute containing two... Comprehensions, `for x in []` and `for y in x`. So not only is the term "generator" overloaded here from its usual meaning, but we're also using the term "comprehension" itself to mean two entirely different things, one of which is part of the other 🤯

(To be clear, this is not a critique of this PR, just a lament for the state of naming in this part of the AST. We could fix this in our AST, but then we'd be inventing our own naming scheme that doesn't match the CPython AST, and that doesn't seem great either.)

One way to maybe clarify this naming a bit would be to switch from `list_comprehension`, `dict_comprehension`, etc all through these method names to instead use `listcomp`, `dictcomp`, etc when we are referring to the outer expression, and reserve the full word "comprehension" for the thing actually named `Comprehension` in the AST, which is just the inner `for x in y` part. I wouldn't exactly call that _clear_, but given what we're working with, it might at least be better? It at least matches the names used in the AST and avoids using the word "comprehension" to mean two different things.


In any case, I don't like the names `infer_comprehensions_scope` and `infer_comprehension_scope` for these two methods. The individual "Comprehension" nodes inside a list/set/dict/gen-comp are not different scopes, so the use of "scope" here is misleading.

If we go with my suggestion above to rename e.g. `infer_list_comprehension_expression` to `infer_listcomp_expression`, then I think we could name these two just `infer_comprehensions` and `infer_comprehension`.


---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1663 on 2024-09-03 19:17_

In the parallel code above in `infer_first_comprehension_iter` you changed all the naming from `generator*` to `comprehension*`, which is reasonable since `generators` attribute is a list of `Comprehension` nodes. Let's at least be consistent and use the same naming both here and in `infer_first_comprehension_iter`.

---

_@carljm approved on 2024-09-03 19:29_

Looks good! My only issue is with the name(s) `infer_comprehension(s)_scope`, otherwise this makes sense. Thanks for taking care of this!

---

_@dhruvmanila reviewed on 2024-09-04 04:44_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:159 on 2024-09-04 04:44_

Yeah, that's what I thought as well which is the reason to limit this. It can easily be reverted in the future if there's a need.

---

_@dhruvmanila reviewed on 2024-09-04 04:52_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1656 on 2024-09-04 04:52_

I agree with the sentiment, thanks for sharing that.

> (To be clear, this is not a critique of this PR, just a lament for the state of naming in this part of the AST. We could fix this in our AST, but then we'd be inventing our own naming scheme that doesn't match the CPython AST, and that doesn't seem great either.)

I think, if we want, we _can_ change the names in our AST as we've done so in the past as well (https://github.com/astral-sh/ruff/pull/8064, https://github.com/astral-sh/ruff/pull/6379, https://github.com/astral-sh/ruff/pull/6253, etc.). There are other recommendations here: https://github.com/astral-sh/ruff/discussions/6183

> One way to maybe clarify this naming a bit would be to switch from `list_comprehension`, `dict_comprehension`, etc all through these method names to instead use `listcomp`, `dictcomp`, etc when we are referring to the outer expression, and reserve the full word "comprehension" for the thing actually named `Comprehension` in the AST, which is just the inner `for x in y` part.

From my perspective, both `listcomp` and `list_comprehension` seems equivalent as the former seems like a short version of the latter. Is the reason for recommending `listcomp` because of it's presence in the CPython AST?

> In any case, I don't like the names `infer_comprehensions_scope` and `infer_comprehension_scope` for these two methods. The individual "Comprehension" nodes inside a list/set/dict/gen-comp are not different scopes, so the use of "scope" here is misleading.

Makes sense. I can change this.

---

_Comment by @codspeed-hq[bot] on 2024-09-04 05:06_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/comprehension-multiple-target)

### Merging #13213 will **improve performances by 4.66%**

<sub>Comparing <code>dhruv/comprehension-multiple-target</code> (b72afe7) with <code>main</code> (3c4ec82)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/comprehension-multiple-target` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 1.9 ms | 1.8 ms | +4.66% |


---

_Merged by @dhruvmanila on 2024-09-04 05:48_

---

_Closed by @dhruvmanila on 2024-09-04 05:48_

---

_Branch deleted on 2024-09-04 05:49_

---

_@dhruvmanila reviewed on 2024-09-04 05:51_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1656 on 2024-09-04 05:51_

I've not renamed the `list_comprehension` to `listcomp` as I feel like the "expression" prefix in `list_comprehension_expression` gives a signal that this is different than a simple "comprehension". Regardless, I don't have a strong opinion, so I'm happy to rename it if you think `listcomp` is better.

---

_@carljm reviewed on 2024-09-04 14:20_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1656 on 2024-09-04 14:20_

Yeah, mostly what my suggestion was aiming for was to hew very closely to the AST naming, in hopes that this would at least give some guideposts to readers. But I'm OK with your approach and relying on `_expression` to distinguish.

---
