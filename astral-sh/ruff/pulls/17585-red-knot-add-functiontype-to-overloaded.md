```yaml
number: 17585
title: "[red-knot] Add `FunctionType::to_overloaded`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dhruv/overloaded-function
created_at: 2025-04-23T16:09:36Z
updated_at: 2025-04-23T21:27:07Z
url: https://github.com/astral-sh/ruff/pull/17585
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Add `FunctionType::to_overloaded`

---

_@dhruvmanila_

## Summary

This PR adds a new method `FunctionType::to_overloaded` which converts a `FunctionType` into an `OverloadedFunction` which contains all the `@overload`-ed `FunctionType` and the implementation `FunctionType` if it exists.

There's a big caveat here (it's the way overloads work) which is that this method can only "see" all the overloads that comes _before_ itself. Consider the following example:

```py
from typing import overload

@overload
def foo() -> None: ...
@overload
def foo(x: int) -> int: ...
def foo(x: int | None) -> int | None:
	return x
```

Here, when the `to_overloaded` method is invoked on the
1. first `foo` definition, it would only contain a single overload which is itself and no implementation.
2. second `foo` definition, it would contain both overloads and still no implementation
3. third `foo` definition, it would contain both overloads and the implementation which is itself

### Usages

This method will be used in the logic for checking invalid overload usages. It can also be used for #17541.

## Test Plan

Make sure that existing tests pass.


---

_Label `internal` added by @dhruvmanila on 2025-04-23 16:09_

---

_Label `red-knot` added by @dhruvmanila on 2025-04-23 16:09_

---

_Comment by @github-actions[bot] on 2025-04-23 16:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @dhruvmanila on 2025-04-23 17:32_

---

_Review requested from @carljm by @dhruvmanila on 2025-04-23 17:32_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-04-23 17:32_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-04-23 17:32_

---

_Review requested from @dcreager by @dhruvmanila on 2025-04-23 17:32_

---

_@MichaReiser reviewed on 2025-04-23 19:56_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:6099 on 2025-04-23 19:56_

Is this needed because salsa woudd otherwise incorrectly pick up the inner function and make it a method? 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:6099 on 2025-04-23 20:01_

Interesting. So having the `#[salsa::tracked]` on the impl block causes problems with having a non-query method with a nested query inside it?

---

_@dhruvmanila reviewed on 2025-04-23 20:01_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:6099 on 2025-04-23 20:01_

I'm not sure by "make it a method" but I think it would make it a query because the previous `impl` block is marked with `#[salsa::tracked]` and it seems wasteful to only cache the `.as_ref` call because the inner function is already tracked.

I could possibly avoid marking the inner function as salsa tracked and merge this into the above `impl` block.

---

_@carljm approved on 2025-04-23 20:02_

This looks great!

---

_@MichaReiser reviewed on 2025-04-23 20:08_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:6165 on 2025-04-23 20:08_

Does that mean that we call `to_overloaded` 4 times if a function has 3 overloads (and one impl). 4 times because:

1. `to_overloaded` on the implementation
2. which calls into `to_overloaded` on the last overload
3. which, in turn, calls into `to_overloaded on the second overload
4. which, in turn, calls into `to_overloaded` on the first overload

Or is it at most two `to_overloaded` calls, one for the impl (root) which then calls into the `to_overloaded` for the first overload?

---

_@MichaReiser reviewed on 2025-04-23 20:11_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:6099 on 2025-04-23 20:11_

> I'm not sure by "make it a method" but I think it would make it a query because the previous impl block is marked with #[salsa::tracked] and it seems wasteful to only cache the .as_ref call because the inner function is already tracked.

The `#[salsa::tracked]` on the impl block doesn't make all functions in that block query. Only functions that itself are also marked with `#[salsa::tracked]` will become queries. That's why I think it should be fine to merge the impl blocks.

---

_@dhruvmanila reviewed on 2025-04-23 20:14_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:6165 on 2025-04-23 20:14_

Yes, the former is correct. It will call 4 times.

---

_@dhruvmanila reviewed on 2025-04-23 20:16_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:6165 on 2025-04-23 20:16_

This kind of gives me a thought as to whether this is being wasting resources in terms of memory because we don't really need to cache the calls on the function that isn't the implementation or the last overload (if implementation doesn't exists).

---

_@MichaReiser reviewed on 2025-04-23 20:19_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:6165 on 2025-04-23 20:19_

Only caching the last result would be great. Not only reduces it overall memory consumption. It also removes the need to clone the `overloads` vec multiple times only to add one extra element. 

Would it be possible to rewrite this logic as a loop where we store the current function type that we look up instead of a recursive function call?

---

_@dhruvmanila reviewed on 2025-04-23 20:20_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:6099 on 2025-04-23 20:20_

I think I misunderstood what `#[salsa::tracked]` does on an impl block. I think I can just merge the two impl blocks.

---

_@dhruvmanila reviewed on 2025-04-23 20:20_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:6099 on 2025-04-23 20:20_

Yeah, I'm not sure why I thought of that. I just realized after sending this message. Thanks. I'll change it.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:6157 on 2025-04-23 20:20_

It would be great to document the limitations that the method only returns overloads defined *before* `self` as this may not be immediately obvious from the implementation

---

_@MichaReiser reviewed on 2025-04-23 20:20_

---

_@carljm reviewed on 2025-04-23 20:31_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:6165 on 2025-04-23 20:31_

It should be possible to do this iteratively instead of recursively. It will be less efficient (will repeat work) if we did end up calling it on intermediate overloads too, but it should be somewhat more efficient if we only end up calling it on the last overload. Which I think should usually be the case?

The memory used here is tiny, and there's no cold regression shown; I doubt that overloads will ever be a significant part of our overall performance profile (cold check time or memory). There is a 1% incremental regression, probably due to the new memoized query results; an iterative approach might be able to bring that down if it reduces the number of memos?

I don't think we should spend more than an hour or two right now on reworking this.

---

_@MichaReiser reviewed on 2025-04-23 20:33_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:6165 on 2025-04-23 20:33_

> t should be possible to do this iteratively instead of recursively. It will be less efficient (will repeat work) if we did end up calling it on intermediate overloads too, but it should be somewhat more efficient if we only end up calling it on the last overload. Which I think should usually be the case?

Could we remove the query all-together, considering that they're rare and because `to_overloaded` is only called from `signature` which itself is cached?

---

_@dhruvmanila reviewed on 2025-04-23 20:36_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:6165 on 2025-04-23 20:36_

Yeah, I don't think we would face any regression if we don't cache the `to_overloaded` right now as most of the usages of overloads would come from `signature` which is cached.

Currently, there isn't any usages of `to_overloaded` but that will happen when I add the checks for valid overloads in a follow-up PR. I think I'd prefer to see if removing the cache would cause any regression after that and not now. What do you think?

---

_@carljm reviewed on 2025-04-23 21:09_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:6165 on 2025-04-23 21:09_

There will likely be another upcoming use for dataclass transforms that wouldn't be from within signature? But would also be within a different query. 

---

_@dhruvmanila reviewed on 2025-04-23 21:19_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:6165 on 2025-04-23 21:19_

Yeah, for now I'll just follow-up with an iterative approach instead. I'll merge this PR after expanding the documentation for `to_overloaded` method.

---

_@dhruvmanila reviewed on 2025-04-23 21:19_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:6157 on 2025-04-23 21:19_

Added a note in [`734a708` (#17585)](https://github.com/astral-sh/ruff/pull/17585/commits/734a708fb407b4a1c90c1e2c4e8becea97cedd5d).

---

_Comment by @dhruvmanila on 2025-04-23 21:26_

Merging this PR, I'll follow-up with an iterative approach (instead of the current recursive approach) to collect the overloads. Later, once we have a couple of usages of `to_overloaded` we can check if not caching the method regresses or not.

---

_Merged by @dhruvmanila on 2025-04-23 21:27_

---

_Closed by @dhruvmanila on 2025-04-23 21:27_

---

_Branch deleted on 2025-04-23 21:27_

---
