```yaml
number: 18360
title: "[ty] Create separate `FunctionLiteral` and `FunctionType` types"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/function-separation
created_at: 2025-05-29T01:27:48Z
updated_at: 2025-06-03T14:59:33Z
url: https://github.com/astral-sh/ruff/pull/18360
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Create separate `FunctionLiteral` and `FunctionType` types

---

_Pull request opened by @dcreager on 2025-05-29 01:27_

This updates our representation of functions to more closely match our representation of classes.

The new `OverloadLiteral` and `FunctionLiteral` classes represent a function definition in the AST. If a function is generic, this is unspecialized. `FunctionType` has been updated to represent a function type, which is specialized if the function is generic. (These names are chosen to match `ClassLiteral` and `ClassType` on the class side.)

This PR does not add a separate `Type` variant for `FunctionLiteral`. Maybe we should? Possibly as a follow-on PR?

Part of https://github.com/astral-sh/ty/issues/462

---

_Label `internal` added by @dcreager on 2025-05-29 01:27_

---

_Label `ty` added by @dcreager on 2025-05-29 01:27_

---

_Comment by @github-actions[bot] on 2025-05-29 17:45_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @dcreager on 2025-05-29 19:32_

---

_Review requested from @carljm by @dcreager on 2025-05-29 19:32_

---

_Review requested from @AlexWaygood by @dcreager on 2025-05-29 19:32_

---

_Review requested from @sharkdp by @dcreager on 2025-05-29 19:32_

---

_Review requested from @MichaReiser by @dcreager on 2025-05-29 19:32_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/function.rs`:64 on 2025-05-30 05:50_

Nit: Should we use `1 << 0` similar to `FunctionDecorators` to have a single style for defining bitflags

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/function.rs`:441 on 2025-05-30 05:52_

```suggestion
    #[returns(deref)]
```

---

_@MichaReiser reviewed on 2025-05-30 05:54_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/function.rs`:413 on 2025-05-30 07:59_

This method is now always returning as oppose to using an `Option<...>`. Looking at the implementation, I think for non-overloaded functions the return type would be `([], Some(fn))` and for an overloaded function it would be `([overload1, overload2, ...], ...)` i.e., the difference between the return type would be whether the overload slice is empty or not. Is that correct?

I've a slight preference to keeping it as an `Option<...>` to make the difference between an overloaded and a non-overloaded function explicit. It's also easier to then skip any logic which is not intended for a non-overloaded function like the overload checks (`check_overloaded_functions`).

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/infer.rs`:2329 on 2025-05-30 08:01_

The previous version is only looping over the overloads and excludes checking the implementation while the new version includes the implementation. Is this expected?

---

_@dhruvmanila approved on 2025-05-30 08:07_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/function.rs`:64 on 2025-05-30 14:25_

Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:2329 on 2025-05-30 14:29_

My reading of the comment above suggested that we should look in the implementation, too, particularly this part:

> just use the params of the last seen usage of `@dataclass_transform`

Do you know if we should only look at the overloads? I can change it to be consistent with the previous version if so.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/function.rs`:413 on 2025-05-30 15:20_

My thinking here was that wrapping the result in `Option` doesn't play nicely with `OverloadLiteral` and `FunctionLiteral` now being different types. If this returns `None` for the non-overloaded case, you'd have to then call some other method to get the non-overloaded `OverloadLiteral`. (That name might be unfortunate, since there is still one `OverloadLiteral` for a non-overloaded function.)

If you don't care about whether the function is overloaded or not, that's either because you just want the signature (and `signature` does the right thing for you), or you just want the last definition, regardless of whether that's (a) the only definition of a non-overloaded function, (b) the implementation of an overloaded function, or (c) the last overload of an overloaded function that has no implementation. And for that you can just access the `current_overload` field — though given this discussion, I think that should maybe be renamed to `last_definition`.

---

_@dcreager reviewed on 2025-05-30 15:21_

---

_Comment by @dcreager on 2025-05-30 15:27_

@dhruvmanila's comment also makes me realize that these types are not well structured for specialized generic functions. (And what this is replacing wasn't either!) It is the individual overloads of a function that are generic or not, but we're currently record a list of specializations for the function as a whole! This is correct for the `inherited_generic_context`, which comes from the containing class and does apply to every overload. But it's not correct for specializations of individual overloads.

I'm going to put this back to draft while I have a think about this.

---

_Converted to draft by @dcreager on 2025-05-30 15:27_

---

_@dhruvmanila reviewed on 2025-05-30 15:49_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/infer.rs`:2329 on 2025-05-30 15:49_

Sorry, you're right! I missed one line in the diff where the previous version was looking at `f` directly which would correspond to either the overload implementation or the last overload. And, come to think of it, that would do duplicate work if the decorator was on the last overload and there's no implementation.

---

_@dhruvmanila reviewed on 2025-05-30 16:11_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/function.rs`:413 on 2025-05-30 16:11_

Thank you! This is useful context

> My thinking here was that wrapping the result in `Option` doesn't play nicely with `OverloadLiteral` and `FunctionLiteral` now being different types. If this returns `None` for the non-overloaded case, you'd have to then call some other method to get the non-overloaded `OverloadLiteral`.

I see. Wouldn't we still require `.unwrap` because the non-overloaded function literal would be represented by `Option<OverloadLiteral>` (second element in the tuple)? And, in the `None` case, we could use the `current_overload` that would represent the non-overloaded function literal, right?

That said, I do see the potential downsides of using an `Option<(..., ...)>` as the resulting type. I'm curious to know if you have an example use-case in mind which you have thought of that would benefit from a non-option type?

---

_Comment by @dcreager on 2025-05-30 19:44_

> @dhruvmanila's comment also makes me realize that these types are well structured for specialized generic functions. (And what this is replacing wasn't either!) It is the individual overloads of a function that are generic or not, but we're currently record a list of specializations for the function as a whole! This is correct for the `inherited_generic_context`, which comes from the containing class and does apply to every overload. But it's not correct for specializations of individual overloads.

We discussed this in Discord, and I think that the current representation is okay.

I think there are still [open questions](https://discuss.python.org/t/pep-718-subscriptable-functions/28457/45) about what it would even mean to explicitly specialize an overloaded function. Until those questions are addressed, we don't need to worry about it, since we don't persist specializations of generic functions. We infer specializations of individual overloads as part of calling one, but we use those specializations immediately to infer the return type, and then throw it away. The only way that specialized functions show up persistently is via a containing generic class (e.g. `C[int].method`), and those are handled correctly by the current representation.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/function.rs`:413 on 2025-05-30 20:17_

> That said, I do see the potential downsides of using an `Option<(..., ...)>` as the resulting type. I'm curious to know if you have an example use-case in mind which you have thought of that would benefit from a non-option type?

It's more that I don't think we need the extra `Option` wrapper to tell you if the function is overloaded, since `overloads.is_empty()` tells you that, e.g.

https://github.com/astral-sh/ruff/blob/86eba968d99e66eac9f7ce3785563afca2bb3777/crates/ty_python_semantic/src/types/infer.rs#L1134-L1137

(I had forgotten that check in the draft you were looking at before, though I think it doesn't add any incorrectness to perform the check on _all_ functions — if a function isn't overloaded, the "check all overloads" loops will not have anything to loop over!)

I also did the renaming I suggested above, so if you don't care if the function is overloaded, and just want the last definition, you can call `function.last_definition(db)` and not have to worry about `overloads_and_implementation` at all.

I also added a big module comment trying to explain this context more. lmkwyt

---

_@dcreager reviewed on 2025-05-30 20:37_

---

_Marked ready for review by @dcreager on 2025-05-30 20:37_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/display.rs`:260 on 2025-05-30 21:31_

This TODO is confusing me. The "when generic function types are supported" part of it looks kind of obsolete -- haven't they been supported for a while now?

But it does look like we still don't show the generic context here, so maybe the overall TODO is still relevant? Is there something blocking us from resolving this? (If not, that doesn't mean it needs to be resolved in this PR.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/function.rs`:5 on 2025-05-30 21:33_

And individual overloads of a given function can independently be generic or not-generic, too.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/function.rs`:18 on 2025-05-30 21:37_

What is the actual TODO here? Is it to stop representing these as `Type::Callable` with the `is_function_like` flag that @sharkdp added, and instead include them in our function representation?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/function.rs`:338 on 2025-05-30 21:54_

I find this comment slightly vague/confusing. This field is part of the identity of a `FunctionLiteral` type, so it kind of doesn't make sense to say "if... being used to..." -- this isn't an ephemeral thing that only shows up when the function is used in a certain way. Can we be more explicit/clear here about precisely under what circumstances we will have an `inherited_generic_context` on a function literal?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/function.rs`:260 on 2025-05-30 22:12_

I think this doc comment has kind of migrated along with various refactors since I think I originally wrote it -- but looking at the current state of the code, I am not convinced it's fully accurate -- I don't think we actually use this at all for type checking the body of the function.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/function.rs`:448 on 2025-05-30 22:13_

This doc comment I think also deserves the same warning about not calling it cross-file. (I guess the fact that it's private should help here.) It's important that all cross-file access goes through the Salsa-tracked `FunctionType::signature`.

---

_@carljm approved on 2025-05-30 22:16_

Pro refactoring: zero tests touched!

---

_@dhruvmanila reviewed on 2025-06-02 03:51_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/function.rs`:413 on 2025-06-02 03:51_

> (I had forgotten that check in the draft you were looking at before, though I think it doesn't add any incorrectness to perform the check on _all_ functions — if a function isn't overloaded, the "check all overloads" loops will not have anything to loop over!)

Got it, thanks for updating.

> I also added a big module comment trying to explain this context more. lmkwyt

This reads well, thank you for adding that!

---

_@dhruvmanila reviewed on 2025-06-02 03:54_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/function.rs`:359 on 2025-06-02 03:54_

nit: I'd prefer to just inline this as I think it's only being used once? Or, if you have any other use cases in mind, feel free to keep it.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/display.rs`:260 on 2025-06-02 17:24_

Nothing blocking other than deciding whether it's what we want to do.  I've reworded the TODO comment for now

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/function.rs`:5 on 2025-06-02 17:25_

Updated

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/function.rs`:18 on 2025-06-02 17:30_

Yes, this comment https://github.com/astral-sh/ruff/pull/18242#issuecomment-2898173409

It's also somewhat combined with the point above about "known" functions, and the idea we've floated about pulling some of the special-case logic out from `call/bind.rs` and `infer.rs` into hook fields of the function representation.

I can reword or remove this if it's too muddled.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/function.rs`:260 on 2025-06-02 17:32_

That's a good point, the signature is only used when analyzing call sites. (We do use the parameter and return type annotations when type-checking the body, but we don't get those from the function's `Signature`)

Removed

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/function.rs`:338 on 2025-06-02 17:37_

I'll answer the question here to help figure out what change to make to the comment.

For something like

```py
class C[T]:
    def __init__(self, x: T) -> None: ...
```

when we encounter `__init__` during type inference, we infer a `FunctionLiteral` for the definition. But we don't check right then whether the method is a generic class constructor, and so (like all other function definitions), this field is `None`:

https://github.com/astral-sh/ruff/blob/f379eb6e624e03583bd23ca3a9b3bc53cf1448a3/crates/ty_python_semantic/src/types/infer.rs#L2038

During _member lookup_ (which is performed as part of class construction), we special-case `__init__` and `__new__` to _update_ this field to hold the generic context of the containing class:

https://github.com/astral-sh/ruff/blob/f379eb6e624e03583bd23ca3a9b3bc53cf1448a3/crates/ty_python_semantic/src/types/class.rs#L1285-L1288

That means we end up creating a _second_ `FunctionLiteral` for the method, which is what we pass on to the call binding logic, to let us infer a specialization of a class.

So I think it actually _is_ an ephemeral thing that only shows up during a constructor call.

Maybe it's enough to reword it as "when this function is a class method [etc]"?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/function.rs`:359 on 2025-06-02 17:40_

Yep good call

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/function.rs`:448 on 2025-06-02 17:46_

Added.  I also added a similar comment to `OverloadLiteral::signature` above, which is crate-public, since it is called by the `display()` implementation, and when rendering diagnostics (which I think are both okay places to add the cross-file dependency?)

---

_@dcreager reviewed on 2025-06-02 17:49_

---

_@dcreager reviewed on 2025-06-03 14:45_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/function.rs`:338 on 2025-06-03 14:45_

Added a summary of :point_up: to the field comment

---

_Merged by @dcreager on 2025-06-03 14:59_

---

_Closed by @dcreager on 2025-06-03 14:59_

---

_Branch deleted on 2025-06-03 14:59_

---
