```yaml
number: 18350
title: "[ty] Split `Type::KnownInstance` into two type variants"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/singletons
created_at: 2025-05-28T10:53:29Z
updated_at: 2025-06-03T18:27:52Z
url: https://github.com/astral-sh/ruff/pull/18350
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Split `Type::KnownInstance` into two type variants

---

_Pull request opened by @AlexWaygood on 2025-05-28 10:53_

## Summary

This PR splits `Type::KnownInstance` into two `Type` variants.

Although the variants of `KnownInstanceType` are more similar to each other than they are to any other `Type` variants, there's nonetheless a blurring of two somewhat distinct concepts in the enum as it currently stands:
- Most variants represent a single symbol that will always exist at one (or possibly two, if it's backported via `typing_extensions`) known locations at runtime. For example: `typing.Required`, `typing.ClassVar`, `typing.Final`, etc.
- Four variants, however, are special enough that they could arguably each have their own `Type` variant. I don't think that would be worth the extra branches everywhere (and the associated maintenance burden), but the point stands that these each represent distinct concepts to the other variants in `KnownInstanceType`. These variants are `KnownInstanceType::TypeAliasType()` (used for all PEP-695 `type` statements), `KnownInstanceType::TypeVar()`, `KnownInstanceType::Generic()` and `KnownInstanceType::Protocol()`.

This PR therefore splits the `Type::KnownInstance` variant into two, and splits the `KnownInstanceType` enum into two. `Type::KnownInstance()` continues to wrap associated data of type `KnownInstanceType`, but `KnownInstanceType` now only holds four variants (the four very special variants described above). `Type::SpecialForm` is added in this PR, and it wraps associated data of type `SpecialFormType`. All variants except the four special ones are moved to the new `SpecialFormType` enum.

The vast majority of variants previously on `KnownInstanceType` are now on `SpecialFormType`, and the refactor here enables the implementation of `SpecialFormType` to be significantly simpler than the previous version of `KnownInstanceType` was:
- Because no variant wraps any associated data, `SpecialFormType` can now implement `Display` directly rather than having to have a `display()` method that takes a `db`. This simplifies the construction of many diagnostics.
- Because no variant wraps any associated data, we can now use `strum_macros::EnumString` to autogenerate the `FromStr` deserialization used in `SpecialFormType::try_from_file_and_name()`. Previously this had to be written out by hand due to some variants wrapping data, but we know from experience that it's hard to remember to keep methods like this up to date (and the compiler can't enforce exhaustiveness over a `match` where the enum variants are on the right-hand side of the `match`).
- `SpecialFormType` no longer needs a `normalized()` method: now that no variants wrap any data, they all just normalize to themselves.

## Test Plan

- Existing tests pass
- This should be a pure refactor with no mypy_primer diff


---

_Label `internal` added by @AlexWaygood on 2025-05-28 10:53_

---

_Label `ty` added by @AlexWaygood on 2025-05-28 10:53_

---

_Comment by @github-actions[bot] on 2025-05-28 10:56_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-05-28 11:07_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-28 11:07_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-28 11:07_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-28 11:07_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-05-28 11:07_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1370 on 2025-05-28 15:34_

nit: Update comment since `typing.Type` is a `SpecialForm` now

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:2349 on 2025-05-28 15:35_

update comment

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:5742 on 2025-05-28 15:38_

Was there a rationale for moving this back into `types.rs`? All else equal, I quite liked how we were moving things into dedicated modules, to help reduce the overwhelming size of this file.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:5684 on 2025-05-28 15:41_

Are the parts about the number of inhabitants true here? Each call to `typing.TypeVar`, for instance, produces a distinct runtime object. That's why there's associated data — to distinguish the different runtime objects and how they behave differently.

(The part about "heavy special-casing" is definitely true, and describes well why this type variant exists)

---

_@dcreager approved on 2025-05-28 15:46_

I very much like not having to thread the `db` through in as many places as before

---

_@AlexWaygood reviewed on 2025-05-28 16:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5684 on 2025-05-28 16:28_

hmm, yes, you're right -- I didn't rewrite this sufficiently from the wording of the `KnownInstanceType` docstring on `main`. Will fix.

---

_@AlexWaygood reviewed on 2025-05-28 16:29_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5742 on 2025-05-28 16:29_

Oh, I'm definitely in favour of continuing to reduce the size of `types.rs`! I mainly put this here because it didn't seem big enough anymore to deserve its own module, and I wasn't sure it really had enough in common with `SpecialFormType` to share a module with that enum. (What would we call the module?)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:520 on 2025-05-28 16:43_

Can we add a docstring here summarizing what this variant represents?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:5699 on 2025-05-28 16:57_

These two docstrings must be wrong (or at least incomplete -- I think they only cover the case of wrapping `None`), because "the symbol `typing.Protocol`" and "the symbol `typing.Generic`" should be simple singleton (barring `typing_extensions`) SpecialForms that don't need to wrap any data. So what all can these variants represent, when they do and when they don't wrap a `GenericContext`?

(I'm not entirely convinced that having these wrap `Option<GenericContext>` is the clearest representation, now that you've done the `SpecialForm` vs `KnownInstance` split. It feels like maybe `typing.Protocol` and `typing.Generic` _should_ be simple `SpecialForm` (how are they different from other `SpecialForm`?) and parametrized `Protocol` and `Generic` should be represented here, wrapping a non-optional `GenericContext`.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:5742 on 2025-05-28 16:58_

Perhaps the same thing it was called before, `known_instance.rs`?

---

_@carljm approved on 2025-05-28 17:06_

It gives me slight pause how many of the cases need to be the same for `SpecialForm` and `KnownInstance`, but overall I do think the simplification and clarification of `SpecialForm` is worth it. Nice work!

---

_@AlexWaygood reviewed on 2025-05-28 17:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5742 on 2025-05-28 17:19_

put them both in `known_instance.rs`? Or put them in separate modules: `SpecialFormType` in `special_form.rs` and `KnownInstanceType` in `known_instance.rs`?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5699 on 2025-05-28 17:21_

> (I'm not entirely convinced that having these wrap `Option<GenericContext>` is the clearest representation, now that you've done the `SpecialForm` vs `KnownInstance` split. It feels like maybe `typing.Protocol` and `typing.Generic` _should_ be simple `SpecialForm` (how are they different from other `SpecialForm`?) and parametrized `Protocol` and `Generic` should be represented here, wrapping a non-optional `GenericContext`.)

that's a great idea! I didn't love this either, but didn't have the "inspiration" to think of this as a solution :-)

---

_@AlexWaygood reviewed on 2025-05-28 17:21_

---

_@AlexWaygood reviewed on 2025-05-29 13:25_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5742 on 2025-05-29 13:25_

(Landing this, but happy to address this in a followup!)

---

_@dcreager reviewed on 2025-05-29 13:30_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:5742 on 2025-05-29 13:30_

(Happy with a followup) I like your suggestion of separate modules `special_form` and `known_instance`

---

_Merged by @AlexWaygood on 2025-05-29 13:47_

---

_Closed by @AlexWaygood on 2025-05-29 13:47_

---

_Branch deleted on 2025-06-03 18:27_

---
