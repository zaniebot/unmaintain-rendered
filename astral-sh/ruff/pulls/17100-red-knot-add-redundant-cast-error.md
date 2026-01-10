```yaml
number: 17100
title: "[red-knot] Add redundant-cast error"
type: pull_request
state: merged
author: manzt
labels:
  - ty
assignees: []
merged: true
base: main
head: manzt/redunant-cast
created_at: 2025-03-31T18:38:08Z
updated_at: 2025-04-01T00:39:57Z
url: https://github.com/astral-sh/ruff/pull/17100
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Add redundant-cast error

---

_Pull request opened by @manzt on 2025-03-31 18:38_

## Summary

Following up from earlier discussion on Discord, this PR adds logic to flag casts as redundant when the inferred type of the expression is the same as the target type. It should follow the semantics from [mypy](https://github.com/python/mypy/pull/1705).

Example:

```python
def f() -> int:
    return 10

# error: [redundant-cast] "Value is already of type `int`"
cast(int, f())
```

~However, I don't think the comparison is quite right yet...there's probably some type comparison behavior I'm misunderstanding. Hopefully it's close enough that the fix is straightforward.~


---

_Review requested from @carljm by @manzt on 2025-03-31 18:38_

---

_Review requested from @AlexWaygood by @manzt on 2025-03-31 18:38_

---

_Review requested from @sharkdp by @manzt on 2025-03-31 18:38_

---

_Review requested from @dcreager by @manzt on 2025-03-31 18:38_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-31 18:38_

---

_Comment by @github-actions[bot] on 2025-03-31 18:41_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser reviewed on 2025-03-31 18:47_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:896 on 2025-03-31 18:47_

Nit: This should probably be `Warn` by default
```suggestion
        default_level: Level::Warning,
```

---

_@manzt reviewed on 2025-03-31 18:51_

---

_Review comment by @manzt on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:896 on 2025-03-31 18:51_

Thanks, i couldn't figure out how to assert a warning in the markdown tests.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:401 on 2025-03-31 18:51_

I think we probably want `is_gradual_equivalent_to` here, and add a test using a non-fully-static type, e.g. simplest case we should also see the warning on `cast(Any, function_returning_any())`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:403 on 2025-03-31 18:52_

Why do we need the `redundant_cast_error` variable instead of just pushing the error directly to `overload.errors` here?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:891 on 2025-03-31 18:54_

This is not a good example since we would infer the `1` as type `Literal[1]`, not `int`, and would not flag this as a redundant cast. Better to use an example like `def f() -> int: ...; cast(int, f())`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/directives/cast.md`:31 on 2025-03-31 18:59_

This is failing because when we see `a: int = 10`, we still track that our inferred type for `a` is `Literal[10]`; we don't throw away that information. (We also track that the "declared type" for `a` is `int`, meaning you can't assign anything outside of `int` to it, but that doesn't change our inferred type for it.) So the cast is not seen as redundant because `int` is not equivalent to `Literal[10]`.

Using a function that returns `int` instead of a simple variable would work better as a test (and is a more realistic case anyway.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:1049 on 2025-03-31 19:03_

This is a good implementation, great work figuring it out on your first contribution! But I think our preference is to avoid adding too many hyper-specific cases that apply to just one known function as binding errors.

I think the preferred approach here would be to add a case for `KnownFunction::Cast` out in `TypeInferenceBuilder::infer_call_expression` (where we already have cases for `RevealType` etc), compare the types and emit the diagnostic there.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:896 on 2025-03-31 19:07_

It's actually exactly the same (even though we use `error:` in the assertions). The only way to test the distinction would be to write a test that uses our configuration support to suppress warnings (not sure if our test config support is up to that yet?) and then show that no diagnostic occurs. But I don't think you need to explicitly test that it's a warning here, just make it one.

---

_@carljm reviewed on 2025-03-31 19:07_

This is great, really impressive first PR, thank you! I do have some suggestions, though ;)

---

_@MichaReiser reviewed on 2025-03-31 19:07_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:896 on 2025-03-31 19:07_

It should be the same as for errors. At least, that's what we do for other errors that have a warning severity by default

---

_@manzt reviewed on 2025-03-31 19:23_

---

_Review comment by @manzt on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:891 on 2025-03-31 19:23_

Right, I understand now. Thank you!

---

_@manzt reviewed on 2025-03-31 19:24_

---

_Review comment by @manzt on `crates/red_knot_python_semantic/resources/mdtest/directives/cast.md`:31 on 2025-03-31 19:24_

Ah interesting! I changed the example to a function that returns an `int`.

---

_@manzt reviewed on 2025-03-31 19:26_

---

_Review comment by @manzt on `crates/red_knot_python_semantic/src/types/call/bind.rs`:401 on 2025-03-31 19:26_

Added. thanks!

---

_@manzt reviewed on 2025-03-31 19:27_

---

_Review comment by @manzt on `crates/red_knot_python_semantic/src/types/call/bind.rs`:403 on 2025-03-31 19:27_

Sorry, I was fighting the borrow checker a bit... `overload.parameter_types()` takes an immutable borrow, which I'm not savvy enough to know the workaround.

```sh
❯ cargo test -p red_knot_python_semantic -- mdtest__directives_cast
   Compiling red_knot_python_semantic v0.0.0 (/Users/manzt/github/manzt/ruff/crates/red_knot_python_semantic)
error[E0502]: cannot borrow `overload.errors` as mutable because it is also borrowed as immutable
   --> crates/red_knot_python_semantic/src/types/call/bind.rs:400:33
    |
398 |                           if let [Some(casted_ty), Some(source_ty)] = overload.parameter_types() {
    |                                                                       -------- immutable borrow occurs here
399 |                               if source_ty.is_gradual_equivalent_to(db, *casted_ty) {
400 | /                                 overload
401 | |                                     .errors
402 | |                                     .push(BindingError::RedundantCast { ty: *casted_ty });
    | |_________________________________________________________________________________________^ mutable borrow occurs here
403 |                               }
404 |                               overload.set_return_type(*casted_ty);
    |                                                        ---------- immutable borrow later used here

For more information about this error, try `rustc --explain E0502`.
error: could not compile `red_knot_python_semantic` (lib) due to 1 previous error
``` 

---

_@manzt reviewed on 2025-03-31 19:39_

---

_Review comment by @manzt on `crates/red_knot_python_semantic/src/types/call/bind.rs`:403 on 2025-03-31 19:39_

This is no longer necessary with the changes now in `infer.rs`.

---

_@manzt reviewed on 2025-03-31 19:41_

---

_Review comment by @manzt on `crates/red_knot_python_semantic/src/types/call/bind.rs`:1049 on 2025-03-31 19:41_

Thanks! yeah I just found a way here spelunking into the code base. I see now its a better fit for where you mentioned, and moved the diagnostics to `TypeInferenceBuilder::infer_call_expression`.

---

_@manzt reviewed on 2025-03-31 19:46_

---

_Review comment by @manzt on `knot.schema.json`:614 on 2025-03-31 19:46_

Should this be capitalized? I saw some rule titles are capitalized and others are not.

---

_Review comment by @carljm on `knot.schema.json`:614 on 2025-03-31 19:51_

If we're already inconsistent, then I think it doesn't matter for this PR; we need to pick a preference and run through them to make them consistent either way. I'm not sure what's better at the moment.

---

_@carljm approved on 2025-03-31 19:51_

Looks great!

---

_Comment by @AlexWaygood on 2025-03-31 20:11_

The mypy_primer hits don't look ideal: we may want to special-case `Todo` types so that a "cast to a Todo type" is never reported as redundant

---

_Comment by @manzt on 2025-03-31 20:11_

Sorry... I ran cargo and black on the docs to fix the errors.

---

_Comment by @carljm on 2025-03-31 20:29_

> The mypy_primer hits don't look ideal: we may want to special-case `Todo` types so that a "cast to a Todo type" is never reported as redundant

Oh yeah, good call. We probably should do that before landing this, to avoid adding new false positives. Looks like we need to special case "Todo type" and "union with todo type" to avoid adding new false positives.

---

_Comment by @manzt on 2025-03-31 20:48_

> Looks like we need to special case "Todo type" and "union with todo type" to avoid adding new false positives.

Would this be some check `is_todo_type(source_ty)` or some equivalent within `infer.rs` before deciding to report an error?

---

_Comment by @carljm on 2025-03-31 21:02_

We usually just add methods on `Type`; looks like we already have `Type::is_todo` but might want to add `Type::contains_todo` to cover the union case. Then that can be checked in the new clause you added in `infer_call_expression`.

---

_Comment by @manzt on 2025-04-01 00:27_

> but might want to add Type::contains_todo to cover the union case. 

Ok, I _think_ I was able to implement this. I wasn't sure how to specify an explicity Todo type in `cast.md` to test the case.

---

_@manzt reviewed on 2025-04-01 00:29_

---

_Review comment by @manzt on `crates/red_knot_python_semantic/src/types.rs`:329 on 2025-04-01 00:29_

Not sure if this should internally check `self.is_todo()` and short circuit.

---

_@carljm reviewed on 2025-04-01 00:30_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:329 on 2025-04-01 00:30_

That is what I'd expected, yeah... a todo type "contains todo"

---

_@manzt reviewed on 2025-04-01 00:31_

---

_Review comment by @manzt on `crates/red_knot_python_semantic/src/types.rs`:329 on 2025-04-01 00:31_

Ok right, I figured and pushed the changes already.

---

_Comment by @carljm on 2025-04-01 00:32_

I think it's OK not to add a test for this -- it's not important behavior we want to preserve, it's just a temporary thing to reduce false positives from the ecosystem check. So the ecosystem check itself will avoid regression (and whenever it doesn't see any false positives anymore, then we also don't want to keep this code around anymore.)

---

_Comment by @carljm on 2025-04-01 00:34_

Looks great, thanks! And ecosystem check is now reporting no changes. Setting to auto-merge when CI finishes.

---

_Merged by @carljm on 2025-04-01 00:37_

---

_Closed by @carljm on 2025-04-01 00:37_

---

_Branch deleted on 2025-04-01 00:39_

---
