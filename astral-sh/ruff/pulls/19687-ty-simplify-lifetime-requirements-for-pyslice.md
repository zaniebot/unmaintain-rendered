```yaml
number: 19687
title: "[ty] Simplify lifetime requirements for `PySlice` trait"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/pyslice-lifetimes
created_at: 2025-08-01T13:43:41Z
updated_at: 2025-08-01T14:16:05Z
url: https://github.com/astral-sh/ruff/pull/19687
synced_at: 2026-01-12T15:56:45Z
```

# [ty] Simplify lifetime requirements for `PySlice` trait

---

_@AlexWaygood_

## Summary

I ran into some nightmarish borrow-check issues in https://github.com/astral-sh/ruff/pull/19669 that were hard to figure out. This PR fixes them -- but I'm splitting it out of #19669 because it seems like a nice simplification on its own merits (it simplifies all callsites of the `py_slice` method), and helps reduce the diff to review for that PR.

I had to enlist @oconnor663's help to understand the lifetime issues I was running into in #19669. Quoting his analysis:

> the problem was the signature of `PySlice::py_slice`, specifically its `&'db` self parameter. in our case, `tuple` is a borrowed local variable, and this is saying that that borrow needs to live for `'db`, which is impossible.
>
> that lifetime bound is there because we're returning `impl Iterator<Item = &'db Self::Item>`. the references we're returning can't outlive `self`, so we need to get rid of `'db` there too. but that leads to another issue, which I expect was the motivation for sprinkling `'db` around everywhere in the first place.
>
> ```
> error[E0491]: in type `&<Self as PySlice<'db>>::Item`, reference has a longer lifetime than the data it references
>    --> crates/ty_python_semantic/src/util/subscript.rs:119:31
>     |
> 119 |     ) -> Result<impl Iterator<Item = &Self::Item>, StepSizeZeroError>;
>     |                               ^^^^^^^^^^^^^^^^^^
> ```
>
> I think the wording of that error is misleading. It's not that we're saying the lifetime of `&self` is longer than `'db`, it's just that those are two independent lifetimes, and we haven't said anything about how they relate. We know that `&self` is short lived, and the type system can see that in our impl blocks, because there `'db` is a type parameter of Self (which I think implicitly requires that `Self` doesn't outlive `'db`). But in the trait definition there's nothing saying that `Self` is bound by `'db`.

@oconnor663 has an alternative patch at https://github.com/astral-sh/ruff/commit/36a8edaf4bc71f886dfefa8cbe356005d618d082 that also fixes these issues, and continues the behaviour on `main` whereby `PySlice` is implemented for all `[T]` (this PR changes this: `PySlice` is now only implemented for all `[T]` where `T: Copy`). I _weakly_ prefer my version here, because it simplifies all callsites of the trait method, and iterating over references to `Type`s feels a _bit_ silly when we know they're all `Copy`. But I'm happy to go with Jack's solution too, if we'd prefer to continue implementing the trait for `[T]` where `T: !Copy`!

## Test Plan

`cargo build`


---

_Review requested from @carljm by @AlexWaygood on 2025-08-01 13:43_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-01 13:43_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-01 13:43_

---

_Label `internal` added by @AlexWaygood on 2025-08-01 13:43_

---

_Label `ty` added by @AlexWaygood on 2025-08-01 13:43_

---

_Comment by @github-actions[bot] on 2025-08-01 13:45_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @sharkdp on 2025-08-01 13:52_

I think I wrote that code initially. Sorry that it caused problems. I vaguely remember seeing/thinking about those problems back then, but I don't have the full context anymore. Seems like two people are involved already, so I'll leave it up to you two to decide whichever solution you prefer.

---

_Comment by @github-actions[bot] on 2025-08-01 13:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review requested from @oconnor663 by @AlexWaygood on 2025-08-01 13:53_

---

_Comment by @oconnor663 on 2025-08-01 14:10_

No preference here about which fix to take. Returning "stuff" instead of "references to stuff" feels like a simplicity win to me too, if we can get away with it.

---

_@oconnor663 approved on 2025-08-01 14:11_

LGTM as a lifetime fix, though maybe someone who knows this code better might have an opinion about the "slices of non-Copy types" question?

---

_Comment by @AlexWaygood on 2025-08-01 14:13_

> though maybe someone who knows this code better might have an opinion about the "slices of non-Copy types" question?

I think the only person who might have a strong opinion is @sharkdp as the original author of this trait, and he's left it up to us 😆

The only current users of the trait are `[Type<'db>]`, `[u8]` and `[char]`: `Type<'db>`, `u8` and `char` are all `Copy`. I can't see why we'd want to use it with a `[T]` where `T: !Copy`, and I think we can easily revisit this question if that does come up as an annoyance in the future.

---

_Merged by @AlexWaygood on 2025-08-01 14:13_

---

_Closed by @AlexWaygood on 2025-08-01 14:13_

---

_Branch deleted on 2025-08-01 14:13_

---

_Comment by @oconnor663 on 2025-08-01 14:15_

![tenor](https://github.com/user-attachments/assets/bfd7a8d7-3427-4345-90cc-4e6c2b1ce7ed)

---
