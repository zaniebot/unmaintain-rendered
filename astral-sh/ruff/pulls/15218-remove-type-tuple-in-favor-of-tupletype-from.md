```yaml
number: 15218
title: "Remove `Type::tuple` in favor of `TupleType::from_elements`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/remove-redundant-type-tuple
created_at: 2025-01-02T08:53:31Z
updated_at: 2025-01-02T16:22:39Z
url: https://github.com/astral-sh/ruff/pull/15218
synced_at: 2026-01-12T15:55:50Z
```

# Remove `Type::tuple` in favor of `TupleType::from_elements`

---

_@sharkdp_

## Summary

Remove `Type::tuple` in favor of `TupleType::from_elements`, avoid a few intermediate `Vec`tors. Resolves an old [review comment](https://github.com/astral-sh/ruff/pull/14744#discussion_r1867493706).

## Test Plan

—


---

_Label `red-knot` added by @sharkdp on 2025-01-02 08:53_

---

_Review requested from @carljm by @sharkdp on 2025-01-02 08:53_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-02 08:53_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-02 08:53_

---

_@AlexWaygood approved on 2025-01-02 08:59_

---

_@MichaReiser approved on 2025-01-02 08:59_

---

_Comment by @github-actions[bot] on 2025-01-02 11:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:3732 on 2025-01-02 12:01_

I'm not a 100% if here is the right place to fix this because `from_elements` could also be used from non-inference code. Maybe it's better to keep the collect in `infer.rs`? 

If you decide to keep it here. I then suggest that we avoid copy over the `elements` if we've seen any `Never` type
```suggestion
                // We can not return early here because we need to consume the whole iterator,
                // to ensure that all element types are inferred.
                let _ = types.last();
                return Type::Never
```

This probably requires using a `while let` 

---

_@MichaReiser reviewed on 2025-01-02 12:01_

---

_@sharkdp reviewed on 2025-01-02 12:12_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3732 on 2025-01-02 12:12_

> I'm not a 100% if here is the right place to fix this because `from_elements` could also be used from non-inference code.

To be clear: I'm assuming you are only referring to a potential performance implication — not some correctness argument?

My thinking was that I'd rather remove this footgun alltogether as opposed to squeezing out a tiny bit of performance: when do we really gain something — performance-wise — by returning early here? Only if we have a very long tuple type where one of the first elements is `Never`. This is an extremely unlikely case that I don't think is worth optimizing for?

---

_@AlexWaygood reviewed on 2025-01-02 12:12_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3732 on 2025-01-02 12:12_

> I'm not a 100% if here is the right place to fix this because `from_elements` could also be used from non-inference code. Maybe it's better to keep the collect in `infer.rs`?

I agree, I think it's best to keep the collect in `infer.rs` and keep the early return here. It seems better for a clean separation of concerns.

Could we also add an mdtest that reproduces the regression that was nearly introduced here? That way we won't accidentally lose test coverage if the fixture file in `crates/ruff_linter` is changed for some unrelated reason

---

_@AlexWaygood reviewed on 2025-01-02 12:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3732 on 2025-01-02 12:16_

> My thinking was that I'd rather remove this footgun alltogether

That's a reasonable point... it just still feels to me like it's the wrong place to be fixing the footgun. Conceptually, the code in `types.rs` should be "pure code" that operates on types and produces other types, rather than code that is concerned with inferring types from ASTs

---

_@MichaReiser reviewed on 2025-01-02 14:00_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:3732 on 2025-01-02 14:00_

That's a fair point. An alternative would be to change the signature to accept a `&[Type<'db>]` which, would remove the need to collecting the types and remove `from_elements` to a simple check if the slices contains `Never`.

```
		if types.iter().any(Type::is_never) {
            return Type::Never;
        }

        Type::Tuple(Self::new(db, elements))
```

---

_@sharkdp reviewed on 2025-01-02 14:20_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3732 on 2025-01-02 14:20_

I saw this too late and changed it back to using `.collect` in `infer.rs`. I added a comment and a regression test. I also manually reviewed all other occurrences of `TupleType::from_elements` and I *think* they are not problematic. But of course, there is no guarantee that we wont run into this again.

I do fully agree with your point that this has actually nothing to do with `TupleType::from_elements` but is more of a general problem in `infer.rs`. We need to be careful everywhere, and I was not, when removing the `.collect` in `infer_tuple_expression`.

---

_Review comment by @AlexWaygood on `crates/red_knot_workspace/resources/test/corpus/88_regression_tuple_type_short_circuit.py`:14 on 2025-01-02 14:40_

For a more realistic function that would be reasonably annotated as returning `Never`, you could do

```suggestion
    while True:
        pass
```

But it might be more concise to create the tuple in a function, where the function has a parameter annotated with `Never`

---

_@AlexWaygood approved on 2025-01-02 14:41_

---

_@sharkdp reviewed on 2025-01-02 15:23_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/resources/test/corpus/88_regression_tuple_type_short_circuit.py`:14 on 2025-01-02 15:23_

> For a more realistic function that would be reasonably annotated as returning `Never`

What's wrong with my function? I believe it is correctly typed? It's also an infinite loop, just with recursion.

> it might be more concise to create the tuple in a function, where the function has a parameter annotated with `Never`

I didn't do that on purpose. A function that accepts `Never` can not be called (as there is no way to produce an element of type `Never`). So we might later detect its body to be unreachable code?

---

_@AlexWaygood reviewed on 2025-01-02 15:29_

---

_Review comment by @AlexWaygood on `crates/red_knot_workspace/resources/test/corpus/88_regression_tuple_type_short_circuit.py`:14 on 2025-01-02 15:29_

> > For a more realistic function that would be reasonably annotated as returning `Never`
> 
> What's wrong with my function? I believe it is correctly typed?

There's nothing wrong with it at all from a typing perspective! But at runtime, it would obviously cause infinite recursion and not be particularly useful 😆 the more common situation in which a function is annotated as returning `Never` or `NoReturn` in my experience is when a function unconditionally raises an exception, or has an infinite loop.

So this is a very minor nitpick, and I certainly don't object if you ignore it and merge your PR as it is! I just think that as a general principle, it's nice if our test snippets emulate realistic Python code as much as possible :-)

---

_@sharkdp reviewed on 2025-01-02 16:21_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/resources/test/corpus/88_regression_tuple_type_short_circuit.py`:14 on 2025-01-02 16:21_

I'm afraid I don't really understand why a function returning `Never` with an infinite (no op) loop is more realistic / useful than one with infinite recursion?

You mean if someone *actually* wants to run this, they would prefer an infinite loop because the recursive version would run into "max recursion depth exceeded"? Okay :smile: 

(Note that this is just a corpus test, not a Markdown test)

> I certainly don't object if you ignore it and merge your PR as it is

:+1: 

---

_Merged by @sharkdp on 2025-01-02 16:22_

---

_Closed by @sharkdp on 2025-01-02 16:22_

---

_Branch deleted on 2025-01-02 16:22_

---
