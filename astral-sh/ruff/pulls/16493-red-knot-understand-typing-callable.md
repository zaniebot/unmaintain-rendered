```yaml
number: 16493
title: "[red-knot] Understand `typing.Callable`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/callable-type
created_at: 2025-03-04T09:16:33Z
updated_at: 2025-03-11T09:08:44Z
url: https://github.com/astral-sh/ruff/pull/16493
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Understand `typing.Callable`

---

_Pull request opened by @dhruvmanila on 2025-03-04 09:16_

## Summary

Part of https://github.com/astral-sh/ruff/issues/15382

This PR implements a general callable type that wraps around a `Signature` and it uses that new type to represent `typing.Callable`.

It also implements `Display` support for `Callable`. The format is as:
```
([<arg name>][: <arg type>][ = <default type>], ...) -> <return type>
```

The `/` and `*` separators are added at the correct boundary for positional-only and keyword-only parameters. Now, as `typing.Callable` only has positional-only parameters, the rendered signature would be:

```py
Callable[[int, str], None]
# (int, str, /) -> None
```

The `/` separator represents that all the arguments are positional-only.

The relationship methods that check assignability, subtype relationship, etc. are not yet implemented and will be done so as a follow-up.

## Test Plan

Add test cases for display support for `Signature` and various mdtest for `typing.Callable`.


---

_Label `red-knot` added by @dhruvmanila on 2025-03-04 09:16_

---

_Comment by @github-actions[bot] on 2025-03-05 10:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:5666 on 2025-03-05 12:59_

This isn't related to this PR and I can separate it out if that's what is preferred.

I think the literals in type expressions are possible only in `Literal` and `Annotated` which are handled in their respective logic. I mainly added this because this way we could raise diagnostic when it's being used in an invalid context i.e., in parameter types of `Callable` (e.g., `Callable[[int, 42], str]`).

TODO: Update the comment that's above this code.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:6079 on 2025-03-05 13:02_

I'm still a bit torn on this. I'm not sure if this is useful as it's still an error case. My main hesitation against this would be that in this scenario if a callable is passed which accepts arguments, it will fail the assignability check. For example,
```py
# This is invalid, we fallback to using `() -> Unknown`
def foo(c: Callable[int]): ...

foo(lambda x, y: None)  # not assignable
```

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:6119 on 2025-03-05 13:02_

Same as above but with return type:

```py
# This is invalid, we fallback to using `() -> str`
def foo(c: Callable[int, str]): ...

foo(lambda x, y: None)  # not assignable
```

---

_@dhruvmanila reviewed on 2025-03-05 13:03_

---

_Marked ready for review by @dhruvmanila on 2025-03-05 17:37_

---

_Review requested from @carljm by @dhruvmanila on 2025-03-05 17:38_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-03-05 17:38_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-03-05 17:38_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-03-05 17:38_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:22 on 2025-03-05 18:22_

Pyright errors on this usage, mypy does not. The typing spec doesn't specify it, so I think it's fine for us to error on it.

Both pyright and mypy infer it as equivalent to `Callable[..., Unknown]`, which I think is better than inferring it as `Unknown`. We don't have to do it in this PR, but I would add a TODO for it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:21 on 2025-03-05 18:24_

One thing I noticed when experimenting in pyright is that apparently `typing.Callable` is deprecated, and `collections.abc.Callable` is now preferred.

I think emitting a deprecation warning for `typing.Callable` is low priority, and we can't support `collections.abc.Callable` until we get star-import support. So I don't think there's any action item here other than to add a TODO for supporting `collections.abc.Callable`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:34 on 2025-03-05 18:30_

I actually think inferring `Callable[int, str]` (or `Callable[42, str]` below, or any similar malformation) as equivalent to `Callable[..., str]` (and of course also emitting an error) is probably better, since it will minimize cascading false positives. If you meant to say `Callable[[int], str]` but you accidentally said `Callable[int, str]`, ideally we would not emit a false positive on every subsequent attempt to call that callable with one argument.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:35 on 2025-03-05 18:33_

This should emit a diagnostic?

Regarding "knowing that `int` is not a paramspec", I think it would be simpler to just make that assumption for now (with a TODO in the code to be addressed when we add `ParamSpec` support), and allow these tests to pass without a TODO.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:40 on 2025-03-05 18:35_

nit: "ParamSpec" is probably clearer than "parameter specification", since it's a term that can be easily searched in the typing spec / docs -- the latter could be understand to have a broader meaning

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:49 on 2025-03-05 18:45_

I don't think these error messages are ideal. Naming the _type_ `Literal[42]` is confusing, because in the context of a type expression, `42` has no meaning at all, it does not spell the type `Literal[42]`.

Also, I think the elements of a parameter list are simply type expressions -- anything valid in a type expression is valid as an item in a parameter list. So it's slightly confusing to say "in parameter list", implying that there is something different about items in a parameter list.

I think ideally we'd use the same error message here that we use if you have e.g. `x: 42`, and it should be something fairly generic that just highlights the invalid type expression and says "[invalid-type-form] Invalid type expression".

(It looks like we currently don't emit any diagnostic at all on `x: 42`, we just infer a declared type of Todo :/ So it may be that fully addressing this comment is out of scope for this PR.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:71 on 2025-03-05 18:49_

Mypy reveals a `...` signature list like this, and Pyright reveals it as `(...)`. I can see arguments in both directions; this version is more explicit, the pyright version is much more concise, and matches the `Callable` syntax. Personally I think I would lean slightly toward the pyright version (so this signature would become `(...) -> Unknown`), because I think it is intuitive and unambiguous and much shorter. But I don't feel strongly, would be curious what @AlexWaygood thinks.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:84 on 2025-03-05 18:51_

I think this is another case (similar to bare `Callable`) where inferring `(...) -> Unknown` over simply `Unknown` would be an improvement with no downside.

The reasoning here is that while clearly the parameters are a mistake, there's no reason to think the use of `Callable` itself was a mistake, so if we can continue to highlight subsequent uses that would not be valid for any `Callable`, those are likely useful and not false positives.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:165 on 2025-03-05 18:56_

Nit: I would not describe these as "explicitly" and "using generics", I would describe it as "legacy" and "PEP 695 syntax". Though what I would actually do is just swap the order and consider the PEP 695 syntax as the default case, which doesn't require special comment. So I'd describe the first one as "Using a ParamSpec in a Callable annotation:" and the second one as "And, using the legacy syntax:"

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:185 on 2025-03-05 18:57_

Similar to above, swap the order and describe the use of `typing_extensions.TypeVarTuple` as "legacy syntax" not "explicitly"

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/unsupported_special_forms.md`:32 on 2025-03-05 18:58_

```suggestion
    # TODO: no error
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/invalid_syntax.md`:107 on 2025-03-05 18:59_

In line with previous comments:
- should probably be `(...) -> Unknown`,  not `() -> Unknown`
- I think we can just make the "not a ParamSpec" assumption for now, until we implement support for `ParamSpec`, and have these TODOs in the code, not the tests

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1269 on 2025-03-05 19:03_

I think this TODO comment belongs with the `Type::Tuple` clause above

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1332 on 2025-03-05 19:04_

I don't think any general callable type is a singleton (for any given signature, there could be any number of distinct objects that are all callable with that signature), so `false` is correct here. We could add some tests for that and remove this TODO.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1379 on 2025-03-05 19:04_

Similarly I don't think any general callable type is single-valued, so `false` is correct here; add a couple tests and remove the TODO.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1415 on 2025-03-05 19:06_

It's ok to leave this as a todo in this PR, but I also think it's quite simple -- we can just defer to a lookup on `object`. The only attributes which can be known to exist on a general callable type are those that exist on `object`.

(The one exception might be that a case could be made for special casing attribute access of `__call__` on a callable. But neither pyright nor mypy do this, so I think we can safely leave that as a TODO indefinitely.

Mypy emits the delicious diagnostic message `error: "Callable[[int], str]" not callable` for this case 😆 )

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1624 on 2025-03-05 19:10_

Same comment as above.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3775 on 2025-03-05 19:13_

As mentioned in comments on the tests, I think an "unknown" callable type should have `*args: Unknown, **kwargs: Unknown` parameters, not empty parameters.

(I realize that above I said "equivalent to `Callable[..., Unknown]` and that's subtly different from what I said here, due to `Any` vs `Unknown`. It should probably be `Unknown`.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:204 on 2025-03-05 19:41_

could also use a `need_slash` flag that starts as `False`, switched to `True` if we see a positional-only, then if its `True` and we see a non-positional-only we emit the `, /` first. In case you think that's clearer. Wouldn't require peeking, and it's a bit more parallel to how you handle adding the `, *`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:556 on 2025-03-05 19:42_

Should probably render this as `() -> Unknown`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:601 on 2025-03-05 19:43_

Might be worth also testing a case where the `/` doesn't go at the end?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:625 on 2025-03-05 19:43_

And a case where the `*` does not go at the start?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5666 on 2025-03-05 19:45_

As discussed in my comment above about the error message, I think this is probably not how we should handle this. I think `Literal` and `Annotated` should be handled in their own way, and we should probably be emitting a diagnostic here immediately, not inferring a type and waiting for `in_type_expression()` to emit a diagnostic on that type.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:6079 on 2025-03-05 19:46_

As discussed in other comments above, I agree with you that we shouldn't have that false positive, but I think the preferable solution is not fall back to fully `Unknown`, but rather that `GeneralCallableType::unknown` should have parameters `(*args: Unknown, **kwargs: Unknown)`, not empty parameters.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:6119 on 2025-03-05 20:04_

Yes, this is the tradeoff we always have to evaluate when deciding what type to infer in error cases: how confident should we be in our interpretation of the apparently-not-wrong parts of the annotation?

I think this case could go either way, since it's possible that `Callable[int, str]` was a typo for `Callable[[int, str], something]`. But that requires two mistakes (omitting brackets, omitting return type), so it seems more likely to me that it's a typo for `Callable[[int], str]`. So I'm comfortable with the current approach here, but open to arguments for changing it.

(If we do change it, I would go for `(...) -> Unknown`, not simply `Unknown` -- the use of `Callable` itself is very unlikely to be a typo.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:106 on 2025-03-05 20:14_

Oh, yeah, maybe we should ideally omit the names here? Not sure how much fallout that change would have

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:33 on 2025-03-05 20:15_

Any reason not to put this down next to the other `Type::Callable` entries?

---

_@carljm reviewed on 2025-03-05 20:15_

Very cool! Great work.

---

_@AlexWaygood reviewed on 2025-03-05 20:44_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:21 on 2025-03-05 20:44_

> One thing I noticed when experimenting in pyright is that apparently `typing.Callable` is deprecated, and `collections.abc.Callable` is now preferred.

Yeah, that's the case for all symbols where the symbol is duplicated between the `typing` and `collections.abc` modules. See https://docs.python.org/3/library/typing.html#deprecated-aliases and https://docs.python.org/3/library/typing.html#typing.Callable and https://docs.python.org/3/library/typing.html#deprecation-timeline-of-major-features

---

_@AlexWaygood reviewed on 2025-03-05 20:49_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:22 on 2025-03-05 20:49_

To me, mypy's behaviour seems more consistent with what the spec mandates for generics (bare list should be inferred as `list[Any]` or `list[Unknown]`) and special forms such as `typing.Tuple` (bare `Tuple` or `tuple` should be inferred as `tuple[Any, ...]`). I also think that going with mypy's behaviour here means it should mean that both former mypy users and pyright users can easily switch to red-knot, whereas if we go with pyright's behaviour then mypy users may have to rewrite some of their `Callable` annotations in order to switch

---

_@AlexWaygood reviewed on 2025-03-05 21:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6119 on 2025-03-05 21:01_

I feel like I'd generally prefer to just infer `Callable[..., Unknown]` for all (or nearly all) malformed `Callable` type expressions. It feels like it involves less guesswork about what the user probably intended. It also feels to me like consistently falling back to the same thing for malformed `Callable` expressions is likely to be much easier to explain to users. It could be pretty confusing if we have one fallback in one error case and another fallback in another case (that may or may not be more precise/accurate, depending on exactly what the mistake was that the user made)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:71 on 2025-03-05 21:11_

I also prefer pyright's display here. The typing spec says that `...` is exactly equivalent to `*args: Unknown, **kwargs: Unknown`, so I suppose mypy's display here is not incorrect (https://typing.python.org/en/latest/spec/callables.html#meaning-of-in-callable). But I nonetheless like that the `...` emphasises that it's not a normal parameter specification — it's a gradual form. The conciseness of pyright's display is also nice

---

_@AlexWaygood reviewed on 2025-03-05 21:11_

---

_@carljm reviewed on 2025-03-05 21:13_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:22 on 2025-03-05 21:13_

I'm ok with not erroring on this for now. It could be an opt-in rule at some point. 

---

_@AlexWaygood reviewed on 2025-03-05 21:15_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:22 on 2025-03-05 21:15_

Oh absolutely! I think we should have an opt-in rule that errors if any type arguments are omitted from any generic class or special form

---

_@carljm reviewed on 2025-03-05 21:16_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:6119 on 2025-03-05 21:16_

I'm OK with that. 

---

_@dhruvmanila reviewed on 2025-03-06 07:03_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:6119 on 2025-03-06 07:03_

This now uses `(...) -> Unknown` signature.

---

_@dhruvmanila reviewed on 2025-03-06 07:04_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:22 on 2025-03-06 07:04_

Makes sense. This now doesn't raise an error and I've added a TODO for the opt-in rule.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:21 on 2025-03-06 07:11_

Added a TODO in markdown content instead of code snippet because it's valid for all `typing` imports.

---

_@dhruvmanila reviewed on 2025-03-06 07:11_

---

_@dhruvmanila reviewed on 2025-03-06 07:21_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:34 on 2025-03-06 07:21_

Yeah, makes sense. Just want to confirm that we still do want to keep the return type intact i.e., use `Callable[..., str]` and not `Callable[..., Unknown]` which is what I've pushed but happy to change it back to using `Unknown`.

---

_@dhruvmanila reviewed on 2025-03-06 07:22_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:35 on 2025-03-06 07:22_

Done

---

_@dhruvmanila reviewed on 2025-03-06 07:23_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:40 on 2025-03-06 07:23_

Yeah, I did a quick search on typing and Python docs and it seems that "parameter specification" would not give users much useful information. I'll change it to use `typing.ParamSpec`

---

_@dhruvmanila reviewed on 2025-03-06 07:36_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:40 on 2025-03-06 07:36_

For context, I mainly used this because `ParamSpec` itself cannot be used directly as type expression similar to `TypeVar` so suggesting `ParamSpec` could be slightly confused by whether to directly use `ParamSpec` v/s a variable that represents the parameter specification.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2703 on 2025-03-06 07:42_

Can we add a small explanation of what needs to be done here or remove the comment if it's redundant to the todo_type description

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:4362 on 2025-03-06 07:43_

```suggestion
		#[return_ref]
    signature: Signature<'db>,
```

We plan to change the default to return a reference to make this less of a footgun but we have to add `return_ref` annotations for now for all fields that aren't `Copy`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:185 on 2025-03-06 07:45_

I think @carljm added a `f.join(", ")` helper that makes it easier to write this kind of formatting logic

---

_@dhruvmanila reviewed on 2025-03-06 07:46_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:49 on 2025-03-06 07:46_

I've changed this to always error at source for bytes, number and boolean literals.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:237 on 2025-03-06 07:47_

The `space` around the `=` should depend on whether there are type annotations to be consistent with the formatter

```py
def test(a=tes, b: int = 4):
    pass
```

---

_@dhruvmanila reviewed on 2025-03-06 07:47_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:84 on 2025-03-06 07:47_

Done

---

_@MichaReiser approved on 2025-03-06 07:49_

---

_@dhruvmanila reviewed on 2025-03-06 07:59_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/invalid_syntax.md`:107 on 2025-03-06 07:59_

I've changed the signature but it would still raise an error because we can't differentiate between `int` and `P` (where `P` is a `ParamSpec`) so the TODO will also exists in tests. Let me know if I'm mis-understanding this.

---

_@dhruvmanila reviewed on 2025-03-06 08:00_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:1269 on 2025-03-06 08:00_

Yeah, earlier it was at the end with both tuple and callable mentioned but as the callable part it elsewhere, I can move it.

---

_@dhruvmanila reviewed on 2025-03-06 08:01_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:3775 on 2025-03-06 08:01_

Yeah, I've used `Unknown` and kept `Any` only for the gradual form (`...`).

---

_@dhruvmanila reviewed on 2025-03-06 08:02_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/display.rs`:204 on 2025-03-06 08:02_

Yeah, I think that's better. We do need to check after the loop for something like `(int, str, /)` because we don't peek but that's ok.

---

_@dhruvmanila reviewed on 2025-03-06 08:04_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/display.rs`:556 on 2025-03-06 08:04_

Yeah, I think that's reasonable.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:34 on 2025-03-06 08:28_

I would prefer `Callable[..., Unknown]` here for the same reasons I stated in https://github.com/astral-sh/ruff/pull/16493#discussion_r1982179486 — it just seems like a simpler policy that involves less guesswork and that will be much easier to explain to users

---

_@AlexWaygood reviewed on 2025-03-06 08:28_

---

_@dhruvmanila reviewed on 2025-03-06 08:32_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:2703 on 2025-03-06 08:32_

I'll expand the comment. I avoided here as it would be similar to `todo_type` but I still grep for `TODO` comments 😅 

---

_@dhruvmanila reviewed on 2025-03-06 08:34_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/display.rs`:185 on 2025-03-06 08:34_

Oh, thanks. I didn't see that.

---

_@dhruvmanila reviewed on 2025-03-06 08:55_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:5666 on 2025-03-06 08:55_

I've updated this to error at source without inferring the types.

---

_@dhruvmanila reviewed on 2025-03-06 08:56_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:6079 on 2025-03-06 08:56_

Updated to use `(...) -> Unknown`.

---

_@dhruvmanila reviewed on 2025-03-06 08:58_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:71 on 2025-03-06 08:58_

Updated to use `...` for gradual form, we'll still show `*args, **kwargs` if they're explicitly provided.

---

_@dhruvmanila reviewed on 2025-03-06 09:01_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:33 on 2025-03-06 09:01_

No reason, I missed this while removing the `todo!()` for the new variant. On that note, I've implemented this to follow the same ordering as other callable types.

---

_@dhruvmanila reviewed on 2025-03-06 09:03_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/signatures.rs`:106 on 2025-03-06 09:03_

Now that we've decided to use `(...) -> Unknown` I don't think this is relevant so I've avoided this.

---

_@dhruvmanila reviewed on 2025-03-06 11:59_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:49 on 2025-03-06 11:59_

Actually, I reverted this back because it otherwise causes errors in other tests and I think it might be best to handle it separately. Sorry for the churn, I'll create an issue to track this.

Edit: https://github.com/astral-sh/ruff/issues/16532

---

_Comment by @dhruvmanila on 2025-03-06 12:04_

### Post review changes

* Use `(...) -> Unknown` consistently
* Display `...` for gradual form. This is done using an additional flag on `Parameters` to indicate the gradual form.
* Use `Any` for gradual form while `Unknown` for invalid forms
* Avoid special casing for param spec variable
* Don't error on bare `Callable`, infer it as `(...) -> Unknown`
* Remove various todos, add a couple more
* Improve display handling of `Signature`

~Regarding the relation implementation, I think I'd prefer to add it in a follow-up PR to keep the changes separate and easier to review.~ I implemented `is_single_valued` and `is_singleton` but not `member` and `static_member` yet.

---

_@dhruvmanila reviewed on 2025-03-06 12:09_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:34 on 2025-03-06 12:09_

Ok, I think I'll also use `Callable[..., Unknown]` for type forms with more than 2 arguments for similar reasons and the fact that we're at least sure that it's a callable object.

---

_Review requested from @carljm by @dhruvmanila on 2025-03-06 15:57_

---

_@dhruvmanila reviewed on 2025-03-07 05:09_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:1415 on 2025-03-07 05:09_

> Mypy emits the delicious diagnostic message `error: "Callable[[int], str]" not callable` for this case 😆 )

Huh, interesting. Even Pyright emits a diagnostics:
```
Pyright: Cannot access attribute "__call__" for class "function"
      Attribute "__call__" is unknown [reportFunctionMemberAccess]
```

If I do a `KnownInstanceType::Callable.static_member(ty, "__call__")`, I get the same error stating that `__call__` does not exists on the type. It does exists on `collections.abc.Callable` and for `typing.Callable` it goes to `_SpecialForm` where it exists as well.

---

_@dhruvmanila reviewed on 2025-03-07 05:13_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:1415 on 2025-03-07 05:13_

Deferring it to `object` lookup also yields the same results that `__call__` is not an attribute.

---

_@dhruvmanila reviewed on 2025-03-07 05:24_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:1332 on 2025-03-07 05:24_

Done

---

_@dhruvmanila reviewed on 2025-03-07 05:24_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:1379 on 2025-03-07 05:24_

Done

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:40 on 2025-03-07 11:45_

I changed it to use ParamSpec directly including others as well. So, it now reads as:
```
The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
```

---

_@dhruvmanila reviewed on 2025-03-07 11:46_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:129 on 2025-03-07 13:35_

I don't think a TODO comment belongs here if the results of the test are accurate and the test doesn't need to change. In other words, "test passing correctly but for the wrong reason" does not merit a TODO in the test, it merits a TODO somewhere in the code. The TODO in the test is just confusing, and is likely to be accidentally left in place even once the test starts passing for the right reason.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/invalid_syntax.md`:104 on 2025-03-07 13:36_

I think this error should not be emitted here, just the previous one (and the syntax error)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1268 on 2025-03-07 13:37_

Also if any parameter type or the return type is not fully static

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1415 on 2025-03-07 13:53_

The `__call__` attribute on `collections.abc.Callable` or `typing.Callable` (or `_SpecialForm`) is not relevant at all here; that's just the behavior of the annotation special form object `Callable` itself, it doesn't have anything to do with the types represented by using `Callable` in an annotation. (This is the difference between `typing.Callable.__call__` vs `def f(c: Callable[..., int]): c.__call__` -- we are talking about the latter in this thread, which is totally different from the former, because `Callable` is a special form, not a regular class with instances.)

> Deferring it to `object` lookup also yields the same results that `__call__` is not an attribute.

Yes, that's expected and correct, because not all Python objects are callable or have a `__call__` method.

Let me try to clarify what I was trying to say in my initial comment here:

1. We should just fall back to `object` members here, because in general there are no attributes that all callable types can be expected to have, except those that all objects have. (It's fine to do this in a separate PR.)
2. The one possible exception to (1) is that we could reasonably say that all callable objects have a `__call__` method. If we implement this, it would _not_ happen naturally as a result of delegating to `object`, it would be a special case we implement here to support `__call__` member on callable types, while delegating to `object` for every _other_ attribute. (It would also be a special case because the signature for the `__call__` method we return would not be read from some general location in typeshed, it just would be the signature wrapped by the callable type itself. If we have a callable type `(int, str) -> int`, its `__call__` method also has that same signature.) But this can be ignored and left as a TODO for now (and probably for some time to come); the fact that neither mypy nor pyright implement `__call__` as a member on callable types suggests that nobody really needs it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3167 on 2025-03-07 13:57_

Can be in a separate PR, but the right answer here is simply `KnownClass::Type.to_instance()`, same as for e.g. `AlwaysFalsy` and `AlwaysTruthy` below -- all three of these are very broad types that can include many different kinds of objects, so unless we are dealing with their one specific characteristic in common (callability, truthiness, falsiness), we don't know anything more about them than we know about `object`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:6420 on 2025-03-07 14:03_

```suggestion
                // TODO: Check whether `Expr::Name` is a ParamSpec
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:150 on 2025-03-07 14:06_

If we give these parameters no name above in `gradual_form`, it seems like it would make sense to also give them no name here?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:40 on 2025-03-07 14:09_

Yeah, I see the potential confusion, where by `ParamSpec` here we mean "a name referring to a ParamSpec", but by `Concatenate` we mean "the actual special form `Concatenate` used here syntactically". I suspect this is fine, though, let's just go with the message as you have it now.

---

_@carljm approved on 2025-03-07 14:16_

Looks good!

---

_@dhruvmanila reviewed on 2025-03-08 03:22_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:129 on 2025-03-08 03:22_

Yeah, thanks for catching that. I've decided to remove the `gradual_form(...)` calls for now as I think it should just go to it's own tests.

---

_@dhruvmanila reviewed on 2025-03-08 03:31_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/invalid_syntax.md`:104 on 2025-03-08 03:31_

This is the case for `Callable[]` where the type argument has a `Name` node but is invalid, it's a limitation on the parser's error recovery. It's similar to point (1) in https://github.com/astral-sh/ruff/issues/10653. So, my preference would be to fix it in the parser instead to avoid emitting the empty `ExprName` node which would then remove the error that's present here.

---

_@dhruvmanila reviewed on 2025-03-08 03:38_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/invalid_syntax.md`:104 on 2025-03-08 03:38_

Hmm, looking at the `ast::ExprSubscript` node, it seems that `slice` field is required (`Box<Expr>`) which is why an empty slice still uses an `Expr` node.

I think it might be ok to check for invalid node here. I'll update it.

---

_Merged by @dhruvmanila on 2025-03-08 03:58_

---

_Closed by @dhruvmanila on 2025-03-08 03:58_

---

_Branch deleted on 2025-03-08 03:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/display.rs`:175 on 2025-03-10 13:43_

nit: it might be better for this just to be

```suggestion
    return_ty: Option<Type<'db>>,
```

since `Type` is `Copy`

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/signatures.rs`:354 on 2025-03-10 13:49_

nit

```suggestion
    pub(crate) const fn is_keyword_only(&self) -> bool {
        matches!(self.kind, ParameterKind::KeywordOnly { .. })
    }

    pub(crate) const fn is_positional_only(&self) -> bool {
```

---

_@AlexWaygood reviewed on 2025-03-10 14:23_

two _tiny_ nits I spotted that probably don't warrant their own PR, but could maybe be addressed if you're doing further followup work on Callable types

---

_@dhruvmanila reviewed on 2025-03-11 06:08_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:1415 on 2025-03-11 06:08_

This was really helpful. Thank you for the explanation.

---

_@dhruvmanila reviewed on 2025-03-11 06:18_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/display.rs`:175 on 2025-03-11 06:18_

Makes sense, thanks.

---

_@dhruvmanila reviewed on 2025-03-11 06:24_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/signatures.rs`:354 on 2025-03-11 06:24_

I might be wrong but I'm not sure if this is going to make any difference as we don't use any of these functions in the const context and I don't know we'll ever be able to (?) because parameters are always going to be dynamic i.e., we cannot statically determine during compile time whether a parameter is positional or not.

---

_@dhruvmanila reviewed on 2025-03-11 09:08_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:3167 on 2025-03-11 09:08_

Done in https://github.com/astral-sh/ruff/pull/16618

---

_@dhruvmanila reviewed on 2025-03-11 09:08_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:1624 on 2025-03-11 09:08_

Done in https://github.com/astral-sh/ruff/pull/16618

---

_@dhruvmanila reviewed on 2025-03-11 09:08_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:1415 on 2025-03-11 09:08_

Done in https://github.com/astral-sh/ruff/pull/16618

---

_@dhruvmanila reviewed on 2025-03-11 09:08_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/display.rs`:175 on 2025-03-11 09:08_

Done in https://github.com/astral-sh/ruff/pull/16618

---
