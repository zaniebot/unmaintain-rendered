```yaml
number: 17400
title: "[red-knot] Add object to intersection if positive is empty"
type: pull_request
state: closed
author: MatthewMckee4
labels:
  - ty
assignees: []
draft: true
base: main
head: fix-empty-intersection-positive
created_at: 2025-04-14T23:45:49Z
updated_at: 2025-04-19T13:59:00Z
url: https://github.com/astral-sh/ruff/pull/17400
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Add object to intersection if positive is empty

---

_Pull request opened by @MatthewMckee4 on 2025-04-14 23:45_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Empty positive can cause some unwanted effects

## Test Plan

Will have to update some regressions


---

_Comment by @github-actions[bot] on 2025-04-14 23:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @MatthewMckee4 on 2025-04-14 23:49_

Okay there are some obvious regressions:
```py
def _(x: Not[int]):
    reveal_type(x) # revealed: object & ~int
```
And just any intersection with no positive now shows this in the reveal type

---

_Comment by @codspeed-hq[bot] on 2025-04-14 23:51_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/MatthewMckee4%3Afix-empty-intersection-positive)

### Merging #17400 will **degrade performances by 9.66%**

<sub>Comparing <code>MatthewMckee4:fix-empty-intersection-positive</code> (5ea428b) with <code>main</code> (312a487)</sub>



### Summary

`❌ 2` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/MatthewMckee4%3Afix-empty-intersection-positive)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 5.4 ms | 5.7 ms | -5.48% |
| ❌ | `` red_knot_micro[many_string_assignments] `` | 97.8 ms | 108.2 ms | -9.66% |


---

_Comment by @MatthewMckee4 on 2025-04-14 23:57_

I think theres an argument to ignore object in the positive if its the only one there for display.
My thinking is that from
https://typing.python.org/en/latest/spec/concepts.html#union-types
we see:
```py
def _(x: object | int):
    reveal_type(x) # revealed: object
```
So we could extend this to `Intersection` like
```py
def _(x: Intersection[object, Not[int]]):
    reveal_type(x) # revealed: ~int
```

From the typing docs:
> If `B` is a subtype of `A`, `B | A` is equivalent to `A`.

For intersection would it make sense to say
If `B` is a subtype of `A`, `B & A` is equivalent to `B`.

And if so i think it is fair to remove object from intersection display like how we do for union display. There could be more to think about here, like it could depend on the negative, but im not sure

---

_@MatthewMckee4 reviewed on 2025-04-14 23:58_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/infer.rs`:5324 on 2025-04-14 23:58_

I think this has caused the codespeed warning

---

_Label `red-knot` added by @AlexWaygood on 2025-04-15 09:24_

---

_Renamed from "Add object to intersection if positive is empty" to "[red-knot] Add object to intersection if positive is empty" by @MatthewMckee4 on 2025-04-15 12:29_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:6242 on 2025-04-15 14:34_

It seems like we shouldn't need this change, if `IntersectionBuilder` always ensures it already.

---

_@carljm reviewed on 2025-04-15 14:35_

Ok, this is too much regression indeed. I suspect it comes from just having to look up `object` in typeshed so often?

Also, I hadn't considered the impact on display of negation types; I agree with you that we don't want to display `object` in every negation type.

---

_Closed by @carljm on 2025-04-15 14:36_

---

_Branch deleted on 2025-04-19 13:59_

---
