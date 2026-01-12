```yaml
number: 14137
title: "[red-knot] Improve `Symbol` API for callable types"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/unbound-methods-follow-up
created_at: 2024-11-06T19:34:56Z
updated_at: 2024-11-07T18:58:34Z
url: https://github.com/astral-sh/ruff/pull/14137
synced_at: 2026-01-12T15:55:46Z
```

# [red-knot] Improve `Symbol` API for callable types

---

_@sharkdp_

## Summary

- Get rid of `Symbol::unwrap_or` (unclear semantics, not needed anymore)
- Introduce `Type::call_dunder`
- Emit new diagnostic for possibly-unbound `__iter__` methods
- Better diagnostics for callables with possibly-unbound / possibly-non-callable `__call__` methods

part of: #14022 

closes #14016

## Test Plan

- Updated test for iterables with possibly-unbound `__iter__` methods.
- New tests for callables

---

_Label `red-knot` added by @sharkdp on 2024-11-06 19:34_

---

_Review requested from @carljm by @sharkdp on 2024-11-06 19:34_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-06 19:34_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-06 19:34_

---

_@sharkdp reviewed on 2024-11-06 19:39_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/symbol.rs`:65 on 2024-11-06 19:39_

Discussion: Arguably, this method is not much better in terms of making its semantics clear. But `get_return_type_if_definitely_bound_and_callable` didn't sound very catchy.

And it is extremely useful for simplifying code at the call sites (three, so far). I tried various other ways of rewriting the part in `infer_binary_expression_type`, and nothing came close to what we have with `get_return_type_if_callable`. The trick is that we intentionally merge two things in the `None` answer: (possible) unboundness and non-callability. This is also why this combines both `.call(…)` and `.return_ty`.

That said, I'm very open to other suggestions.

---

_Comment by @github-actions[bot] on 2024-11-06 19:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/symbol.rs`:57 on 2024-11-06 21:17_

I would just call this return_type_if_callable. Getters are rather uncommon in Rust (set methods are more common but also rear)

---

_@MichaReiser reviewed on 2024-11-06 21:17_

---

_@sharkdp reviewed on 2024-11-06 21:42_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/symbol.rs`:57 on 2024-11-06 21:42_

I had `return_type_if_callable` first, but it could be read as: "return the (inner) type if this symbol is callable" — which is not what it does :man_shrugging:. Due to the dual meaning of "return" here. But I'm happy to select the shorter name.

---

_@MichaReiser reviewed on 2024-11-06 21:44_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/symbol.rs`:57 on 2024-11-06 21:44_

Hmm, I see. How about `callable_return_type` : The callable's return type or possibly just `call_return_type` to resemble `x.call().return_ty`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/symbol.rs`:57 on 2024-11-06 23:26_

Alternative suggestion: just name it `call` :) This is parallel to `Type::call`, and I think it's a reasonable assumption that the return `Type` of a method named `call` is the type returned by the call.

I also think we could improve explicitness and consistency here significantly, while not regressing ergonomics much, if this returned a `CallOutcome` and we added `MaybeNotBound` as a possible `CallOutcome`? Then we can continue to consolidate and improve our APIs on `CallOutcome` (which already includes a method for "get me the return type as Option<Type> or None if error") and consistently return those from all calls. Then call sites which really do want to ignore all reasons for the error can do so with just one more chained method call, but it's a bit more explicit what they are doing?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/symbol.rs`:65 on 2024-11-06 23:26_

See above -- I like this, but my suggestion is to have it return `CallOutcome`, and keep all our logic for "simplifying" call outcomes consolidated in one place as methods on `CallOutcome`.

---

_@carljm approved on 2024-11-06 23:56_

---

_@sharkdp reviewed on 2024-11-07 09:13_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/symbol.rs`:57 on 2024-11-07 09:13_

> I also think we could improve explicitness and consistency here significantly, while not regressing ergonomics much, if this returned a `CallOutcome` and we added `MaybeNotBound` as a possible `CallOutcome`?

I had tried this before, and did so again now. I think it's more explicit, but the disadvantage of this approach is that we now have a few `CallOutcome` variants that are possible results of `Symbol::call`, but not of `Type::call`. Callers which use `Type::call` will now have to (implicitly or explicitly) ignore the possibility of `CallOutcome::Unbound` and similar. This is not a major issue right now, though. A related discussion is #13996, because we now squeeze even more cases through `return_ty` (*what does `None` mean?*) and `return_ty_result` (*under which circumstances will I get a return type?*).

---

_@sharkdp reviewed on 2024-11-07 09:15_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/symbol.rs`:57 on 2024-11-07 09:15_

On the plus side, this new version helped me to find some previously unhandled cases of possible unboundness. See new tests in `call/callable_instance.md`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1213 on 2024-11-07 17:29_

I think this comment can be removed, since this method is no longer handling this; it's part of the semantics of `call_dunder`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1971 on 2024-11-07 17:32_

naming nit: this is really saying the `__iter__` method is possibly unbound, right? So maybe it should be named `PossiblyUnboundIterMethod` or `PossiblyUnboundDunderIter` to parallel the `CallOutcome` variant?

---

_@carljm approved on 2024-11-07 17:32_

This looks good to me, just a couple minor nits! Thanks for pushing through a few different versions of this :)

---

_@carljm reviewed on 2024-11-07 17:37_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1311 on 2024-11-07 17:37_

Should/can we use `call_dunder` here?

---

_@sharkdp reviewed on 2024-11-07 18:24_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1311 on 2024-11-07 18:24_

It's slightly awkward since we only use the boundness information inside the nested conditional, but it's only slightly more code — while being more consistent.

We can actually also use it below for `__get_item__` where it improved the code a lot.

---

_@carljm approved on 2024-11-07 18:40_

Looks good to me!

---

_Merged by @sharkdp on 2024-11-07 18:58_

---

_Closed by @sharkdp on 2024-11-07 18:58_

---

_Branch deleted on 2024-11-07 18:58_

---
