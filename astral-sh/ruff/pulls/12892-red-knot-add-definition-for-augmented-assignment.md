```yaml
number: 12892
title: "[red-knot] Add definition for augmented assignment"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/aug-assign-definition
created_at: 2024-08-14T11:09:52Z
updated_at: 2024-08-20T05:12:21Z
url: https://github.com/astral-sh/ruff/pull/12892
synced_at: 2026-01-12T15:55:42Z
```

# [red-knot] Add definition for augmented assignment

---

_@dhruvmanila_

## Summary

This PR adds definition for augmented assignment. This is similar to annotated assignment in terms of implementation.

~An augmented assignment should also record a use of the variable but that's a TODO for now.~

## Test Plan

Add test case to validate that a definition is added.

---

_Label `red-knot` added by @dhruvmanila on 2024-08-14 11:09_

---

_Comment by @github-actions[bot] on 2024-08-14 11:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dhruvmanila on 2024-08-16 10:56_

---

_Review requested from @carljm by @dhruvmanila on 2024-08-16 10:56_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-16 10:56_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-16 10:56_

---

_@MichaReiser reviewed on 2024-08-16 11:37_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:783 on 2024-08-16 11:37_

I think we also need to insert the type into `types.expressions` to avoid a similar issue to what I fixed in  https://github.com/astral-sh/ruff/pull/12919

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:783 on 2024-08-16 11:37_

ah never mind, I think that works because `infer_expression` is already called below

---

_@MichaReiser reviewed on 2024-08-16 11:37_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:784 on 2024-08-16 11:39_

Is it intentional that we return the target type here and not the value type? Or is it guaranteed that the value and target type are always the same? Is it possible that the return type is different depending on the operator?

---

_@MichaReiser reviewed on 2024-08-16 11:39_

---

_@MichaReiser reviewed on 2024-08-16 11:39_

If this lands after https://github.com/astral-sh/ruff/pull/12919, then we need to add a `HasTy` implementation for `AugmentedAssign` 

---

_@dhruvmanila reviewed on 2024-08-16 12:11_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:783 on 2024-08-16 12:11_

Yes, this wouldn't face the same issue.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:784 on 2024-08-16 12:18_

Not really, it requires additional logic which would be similar to a binary expression to determine the actual type. I'll add a TODO instead and return `Type::Unknown` as is done for other nodes.

---

_@dhruvmanila reviewed on 2024-08-16 12:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:783 on 2024-08-16 13:10_

This isn't really relevant to your PR @dhruvmanila but FWIW, I think this kind of thing is where @carljm's approach of treating `Unbound` as a separate type falls down. If you have a module that just consists of the following, what is the type of `x` -- `int` or `Unbound`?

```py
x += 1
```

At runtime, this is going to fail with `NameError: name 'x' is not defined`, which would imply that `x` should have `Type::Unbound` here. But in an LSP context, we surely want to infer `int` for `x`, since that's obviously what the type of the `x` variable is _meant_ to be, and it'll give us much better results for autocompletion etc. This would be solved if we treated whether or not a variable is unbound as something that can be queried on a `Definition` (`x` would be inferred as an "unbound variable with type `int`" rather than a "variable with type `Unbound`") rather than a separate _type_ altogether.

---

_@AlexWaygood approved on 2024-08-16 13:16_

LGTM.

---

_@carljm reviewed on 2024-08-16 16:19_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:783 on 2024-08-16 16:19_

I think what you're discussing here is orthogonal to representing Unbound as a type or not. In terms of the types that we use for type checking (the types we are confident in), there's no question or issue here: if `x` was unbound before `x += 1`, its still unbound after it (or maybe we prefer switching to unknown in order to avoid more cascading errors, either way can work.) I think actually inferring a type of `int` for `x` in this case, for type checking purposes, is something we definitely don't want to do; we don't want to potentially issue a bunch of cascading downstream type errors because of a type that we just guessed in the face of broken code.

What you're bringing up is a separate feature, which we discussed in planning this morning, which is that in some cases we may have unknown/any as the "correct" type, but we may also want to track a "best guess" type, so we can give "better" (well, hopefully better rather than just misleading) autocomplete etc downstream of incorrect code. That's a feature that needs some design work and discussion, but I don't think representing Unbound as a type makes it any more difficult to implement that feature, or really affects it at all.

---

_@carljm reviewed on 2024-08-16 17:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:783 on 2024-08-16 17:00_

To be clear, I'm very open to considering alternate ways of handling Unbound; I think a good case can be made for other approaches. I just don't think this situation exposes a good rationale for a different approach.

---

_@AlexWaygood reviewed on 2024-08-16 17:02_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:783 on 2024-08-16 17:02_

I think I need to study this more and propose a concrete alternative. I think mypy's approach to this problem is very different; but then, mypy is in general much worse at detecting unbound variables than either pyright or Ruff, so that doesn't say much for mypy's approach! I didn't realise that pyright also treats `Unbound` as a distinct type to `Unknown`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:580 on 2024-08-16 17:05_

Is there a reason to leave this as a TODO rather than just doing it in this PR? It doesn't seem like it should be too involved.

But I'm also fine with doing it as a separate follow-up PR.

---

_@AlexWaygood reviewed on 2024-08-16 17:06_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:783 on 2024-08-16 17:06_

> think mypy's approach to this problem is very different

Ah, I'm wrong again! 😄 <https://github.com/python/mypy/blob/fe15ee69b9225f808f8ed735671b73c31ae1bed8/mypy/types.py#L903>

---

_@carljm approved on 2024-08-16 17:07_

Looks good!

---

_@dhruvmanila reviewed on 2024-08-20 04:58_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:580 on 2024-08-20 04:58_

Yeah, it isn't that much involved. I've updated this PR to implement the TODO.

---

_Merged by @dhruvmanila on 2024-08-20 05:03_

---

_Closed by @dhruvmanila on 2024-08-20 05:03_

---

_Branch deleted on 2024-08-20 05:03_

---
