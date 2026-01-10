```yaml
number: 17145
title: "[red-knot] Fix equivalence of differently ordered unions that contain `Callable` types"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/callable-equivalence-2
created_at: 2025-04-02T10:38:01Z
updated_at: 2025-05-07T15:21:04Z
url: https://github.com/astral-sh/ruff/pull/17145
synced_at: 2026-01-10T18:57:02Z
```

# [red-knot] Fix equivalence of differently ordered unions that contain `Callable` types

---

_Pull request opened by @AlexWaygood on 2025-04-02 10:38_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/17058.

Equivalent callable types were not understood as equivalent when they appeared nested inside unions and intersections. This PR fixes that by ensuring that `Callable` elements nested inside unions, intersections and tuples have their representations normalized before one union type is compared with another for equivalence, or before one intersection type is compared with another for equivalence.

The normalizations applied to a `Callable` type are:
- the type of the default value is stripped from all parameters (only whether the parameter _has_ a default value is relevant to whether one `Callable` type is equivalent to another)
- The names of the parameters are stripped from positional-only parameters, variadic parameters and keyword-variadic parameters
- Unions and intersections that are present (top-level or nested) inside parameter annotations or return annotations are normalized.

Adding a `CallableType::normalized()` method also allows us to simplify the implementation of `CallableType::is_equivalent_to()`.

### Should these normalizations be done eagerly as part of a `CallableType` constructor?

I considered this. It's something that we could still consider doing in the future; this PR doesn't rule it out as a possibility. However, I didn't pursue it for now, for several reasons:
1. Our current `Display` implementation doesn't handle well the possibility that a parameter might not have a name or an annotated type. Callable types with parameters like this would be displayed as follows:
   ```py
   (, ,) -> None: ...
   ```

   That's fixable! It could easily become something like `(Unknown, Unknown) -> None: ...`. But it also illustrates that we probably want to retain the parameter names when displaying the signature of a `lambda` function if you're hovering over a reference to the lambda in an IDE. Currently we don't have a `LambdaType` struct for representing `lambda` functions; if we wanted to eagerly normalize signatures when creating `CallableType`s, we'd probably have to add a `LambdaType` struct so that we would retain the full signature of a `lambda` function, rather than representing it as an eagerly simplified `CallableType`.
2. In order to ensure that it's impossible to create `CallableType`s without the parameters being normalized, I'd either have to create an alternative `SimplifiedSignature` struct (which would duplicate a lot of code), or move `CallableType` to a new module so that the only way of constructing a `CallableType` instance would be via a constructor method that performs the normalizations eagerly on the callable's signature. Again, this isn't a dealbreaker, and I think it's still an option, but it would be a lot of churn, and it didn't seem necessary for now. Doing it this way, at least to start with, felt like it would create a diff that's easier to review and felt like it would create fewer merge conflicts for others.

## Test Plan

- Added a regression mdtest for https://github.com/astral-sh/ruff/issues/17058
- Ran `QUICKCHECK_TESTS=1000000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::stable`


---

_Label `bug` added by @AlexWaygood on 2025-04-02 10:38_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-02 10:38_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-02 10:38_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-02 10:38_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-02 10:38_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2025-04-02 10:39_

---

_Comment by @github-actions[bot] on 2025-04-02 10:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @dhruvmanila on 2025-04-02 15:37_

> Should these normalizations be done eagerly as part of a `CallableType` constructor?

There's some context on this in https://github.com/astral-sh/ty/issues/165 and the linked comment in that issue description.

---

_@carljm reviewed on 2025-04-02 15:42_

This looks good to me. Might be nice for @dhruvmanila to take a look as well?

The tests don't really seem to cover any of the parameter normalization behaviors?

---

_Comment by @AlexWaygood on 2025-04-02 15:46_

> The tests don't really seem to cover any of the parameter normalization behaviors?

In `is_equivalent_to.md`, `f3` and `f4` have equivalent `Callable` supertypes even though their parameters are differently named. The differently named parameters caused the added test to fail previously: the `Callable` supertypes would compare equivalent if you compared them directly, but would not compare equivalent when nested inside unions and intersections. So the tests cover one part of the parameter normalization behaviour; the tests fail without those specific changes.

I should also add a test where there a parameter in a function `X` has a different default value type to a parameter in a function `Y`, though; that's a good point.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:4724 on 2025-04-02 15:47_

Re-using the implementation for `is_equivalent_to` and `is_gradual_equivalent_to` does have the benefit of only looping over the parameters once while the updated implementation in this PR would be doing that multiple times. I have a slight preference of keep using it as the implementation is going to keep existing unless we change `is_gradual_equivalent_to` and it also makes the difference between the two implementation easier to reason about.

---

_@dhruvmanila reviewed on 2025-04-02 16:01_

---

_@dhruvmanila approved on 2025-04-02 16:26_

This looks good apart from one minor comment, thanks!

---

_@AlexWaygood reviewed on 2025-04-02 16:29_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4724 on 2025-04-02 16:29_

That's fair. I think at some future point when we have benchmarks with bigger unions and intersections in them, we might experiment with making `Type::normalized` a Salsa-tracked method, so that once we've done the normalization once we'd never have to do it again. If we do that, it might make sense to go with my new implementation: it'll be slower the first time, but very cheap after that. But we can revisit this if/when we add caching. I'll revert the change to `is_equivalent_to` for now -- thanks for the pushback!

---

_Merged by @AlexWaygood on 2025-04-02 17:43_

---

_Closed by @AlexWaygood on 2025-04-02 17:43_

---

_Branch deleted on 2025-04-02 17:43_

---
