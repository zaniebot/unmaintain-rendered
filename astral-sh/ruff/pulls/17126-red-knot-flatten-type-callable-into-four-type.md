```yaml
number: 17126
title: "[red-knot] Flatten `Type::Callable` into four `Type` variants"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/flatten-callable-types
created_at: 2025-04-01T17:08:20Z
updated_at: 2025-04-01T21:03:40Z
url: https://github.com/astral-sh/ruff/pull/17126
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Flatten `Type::Callable` into four `Type` variants

---

_Pull request opened by @AlexWaygood on 2025-04-01 17:08_

## Summary

Currently our `Type::Callable` wraps a four-variant `CallableType` enum. But as time has gone on, I think we've found that the four variants in `CallableType` are really more different to each other than they are similar to each other:
- `GeneralCallableType` is a structural type describing all callable types with a certain signature, but the other three types are "literal types", more similar to the `FunctionLiteral` variant
- `GeneralCallableType` is not a singleton or a single-valued type, but the other three are all single-valued types (`WrapperDescriptorDunderGet` is even a singleton type)
- `GeneralCallableType` has (or should have) ambiguous truthiness, but all possible inhabitants of the other three types are always truthy.
- As a structural type, `GeneralCallableType` can contain inner unions and intersections that must be sorted in some contexts in our internal model, but this is not true for the other three variants.

This PR flattens `Type::Callable` into four distinct `Type::` variants. In the process, it fixes a number of latent bugs that were concealed by the current architecture but are laid bare by the refactor. Unit tests for these bugs are included in the PR.


---

_Label `red-knot` added by @AlexWaygood on 2025-04-01 17:08_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2025-04-01 17:08_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-01 17:08_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-01 17:08_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-01 17:08_

---

_Comment by @github-actions[bot] on 2025-04-01 17:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-04-01 17:19_

Some other notes:
- This structure more accurately reflects the fact that lots of the `Type` variants have possible inhabitants that would also inhabit `Callable` (instances can be callable, class objects are nearly always callable, same for `type[]` types) -- the distinction implied by the current structure that there are "4 callable types" and all other variants do therefore not represent "callable types" is incorrect
- This doesn't fix https://github.com/astral-sh/ruff/issues/17058, but it should make it easier to fix that issue (at least how I'm currently thinking of fixing it!)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:267 on 2025-04-01 18:11_

```suggestion
    /// The type of an arbitrary callable object with a certain specified signature.
```

---

_@carljm approved on 2025-04-01 18:14_

Nice, yeah I think this structure is less error-prone: avoids grouping very different types together in a way that makes it too easy to wrongly handle them all identically.

---

_Merged by @AlexWaygood on 2025-04-01 18:30_

---

_Closed by @AlexWaygood on 2025-04-01 18:30_

---

_Branch deleted on 2025-04-01 18:30_

---

_Comment by @dhruvmanila on 2025-04-01 19:39_

Thanks Alex for doing this! I still want to take a stab at merging the types to reduce the number of variants but that's something for later.

---

_Comment by @sharkdp on 2025-04-01 21:03_

> I still want to take a stab at merging the types to reduce the number of variants but that's something for later.

It would be great if we could postpone this until after https://github.com/astral-sh/ruff/pull/17017 has been merged; that PR makes major changes to two of these callable types, and rebasing on top of this branch here was a significant amount of work already. But in general, that sounds like a good idea :+1: 

---
