```yaml
number: 16261
title: "[red-knot] Prefix `Type::call` and `dunder_call` with `try`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/rename-type-methods
created_at: 2025-02-19T17:02:58Z
updated_at: 2025-02-20T09:08:28Z
url: https://github.com/astral-sh/ruff/pull/16261
synced_at: 2026-01-10T19:57:23Z
```

# [red-knot] Prefix `Type::call` and `dunder_call` with `try`

---

_Pull request opened by @MichaReiser on 2025-02-19 17:02_

## Summary

This PR prefixes the `Type::call` and `dunder_call` methods with `try` to make it more explicit that they return a `Result` 
and add `call` and `dunder_call` methods that return the return-type without a `Result`. These new methods are intended
to be used outside of type checking. 

This follows an existing pattern:

> We also have Class::try_mro() and Class::mro(), which are similar. And Class::try_metaclass() / Class::metaclass()

The main motivation for this change is https://github.com/astral-sh/ruff/pull/16238 because it is somewhat common to call `bool` outside 
of type checking -- Making it desirable to have to dedicated methods for it. Splitting the methods also helps improving performance
because we can skip some analysis when we don't care about errors.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-02-19 17:02_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-19 17:02_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-19 17:02_

---

_Label `red-knot` added by @MichaReiser on 2025-02-19 17:03_

---

_Renamed from "Prefix `Type::call` and `dunder_call` with `try`" to "[red-knot] Prefix `Type::call` and `dunder_call` with `try`" by @MichaReiser on 2025-02-19 17:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2696 on 2025-02-19 17:32_

Nice catch!

---

_@AlexWaygood reviewed on 2025-02-19 17:32_

The renaming of the existing methods to `Type::try_call()`, `Type::try_call_bound` and `Type::try_call_dunder` makes sense to me. I'm not sure I see the motivation for adding the unused `Type::call()`, `Type::call_dunder` and `Type::call_bound` methods, though. They seem like they should be fairly easy to add if we ever need them?

---

_Comment by @MichaReiser on 2025-02-19 17:38_

I think it helps establish the pattern and it more clearly communicates the idea. It also provides a place to document where they should be used. My last argument is that I'm often too lazy to add methods like this if I have to use them the first time and I only add them if I rewrote the same code like 10 times. Maybe that's a good thing but I want to take that burden from future me. 

But I don't feel strongly. Removing them just means that we'll have some `op` `try_op` pairs whereas other methods don't. Which seems inconsistent (I like symmetry, you know ;))

---

_@AlexWaygood approved on 2025-02-19 17:40_

I lean towards leaving out the unused methods until we need them, since we should usually encourage people to use the methods that return `Result`s in most contexts anyway. But I also don't feel too strongly :-)

---

_@carljm approved on 2025-02-19 20:56_

I like the pattern, but I also think that we should omit methods that we don't currently need (and IMO it is not clear if we will ever need them.) I think the existence of `try_call` already strongly implies the pattern even if `call` doesn't exist (yet).

This will cause more rebase pain for @sharkdp on the descriptors PR :)

---

_Merged by @MichaReiser on 2025-02-20 09:05_

---

_Closed by @MichaReiser on 2025-02-20 09:05_

---

_Branch deleted on 2025-02-20 09:05_

---

_Comment by @github-actions[bot] on 2025-02-20 09:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
