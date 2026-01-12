```yaml
number: 13548
title: "[red-knot] feat: introduce a new `[Type::Todo]` variant"
type: pull_request
state: merged
author: Slyces
labels:
  - ty
assignees: []
merged: true
base: main
head: feat/todo-type
created_at: 2024-09-28T20:17:44Z
updated_at: 2024-09-30T21:32:42Z
url: https://github.com/astral-sh/ruff/pull/13548
synced_at: 2026-01-12T15:55:44Z
```

# [red-knot] feat: introduce a new `[Type::Todo]` variant

---

_@Slyces_

This variant shows inference that is not yet implemented..

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

PR #13500 reopened the idea of adding a new type variant to keep track of not-implemented features in Red Knot.

It was based off of #12986 with a more generic approach of keeping track of different kind of unknowns. Discussion in #13500 agreed that keeping track of different `Unknown` is complicated for now, and this feature is better achieved through a new variant of `Type`.

### Requirements

Requirements for this implementation can be summed up with some extracts of comment from @carljm on the previous PR

> So at the moment we are leaning towards simplifying this PR to just use a new top-level variant, which behaves like Any and Unknown but represents inference that is not yet implemented in red-knot.

> I think the general rule should be that Todo should propagate only when the presence of the input Todo caused the output to be unknown.
>
> To take a specific example, the inferred result of addition must be Unknown if either operand is Unknown. That is, Unknown + X will always be Unknown regardless of what X is. (Same for X + Unknown.) In this case, I believe that Unknown + Todo (or Todo + Unknown) should result in Unknown, not result in Todo. If we fix the upstream source of the Todo, the result would still be Unknown, so it's not useful to propagate the Todo in this case: it wrongly suggests that the output is unknown because of a todo item.

## Test Plan

This PR does not introduce new tests, but it did required to edit some tests with the display of `[Type::Todo]` (currently `@Todo`), which suggests that those test are placeholders requirements for features we don't support yet.


---

_Review requested from @carljm by @Slyces on 2024-09-28 20:17_

---

_Review requested from @MichaReiser by @Slyces on 2024-09-28 20:17_

---

_Review requested from @AlexWaygood by @Slyces on 2024-09-28 20:17_

---

_Comment by @github-actions[bot] on 2024-09-28 20:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:241 on 2024-09-28 23:02_

If you want the second line to be a second sentence, you need a period at the end of the first line ;)

```suggestion
    /// Unknown type (either no annotation, or some kind of type error).
    /// Equivalent to Any, or possibly to object in strict mode
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:244 on 2024-09-28 23:03_

this change isn't correct: this line is part of the same sentence as the previous line

```suggestion
    /// leniency options it could be silently resolved to Unknown in some cases)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:249 on 2024-09-28 23:04_

```suggestion
    /// This variant should eventually be removed once red-knot is spec-compliant.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:254 on 2024-09-28 23:04_

```suggestion
    /// General rule: `Todo` should only propagate when the presence of the input `Todo` caused the
    /// output to be unknown. An output should only be `Todo` if fixing the input to be correctly
    /// inferred would cause the output to be known - this is not the case with `[Type::Any]` or
    /// `[Type::Unknown]`.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/display.rs`:70 on 2024-09-28 23:06_

```suggestion
            // `[Type::Todo]`'s display should be explicit that is not a valid display of
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2236 on 2024-09-28 23:07_

Best not to use `&` here as it could be misinterpreted as meaning "the intersection type of `Any` and `Unknown` 😄

```suggestion
            // When interacting with Todo, Any and Unknown should propagate (as if we fix this Todo
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:5264 on 2024-09-28 23:07_

```suggestion
        // We currently return `Todo` for all `async for` loops,
        // including loops that have invalid syntax
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:5827 on 2024-09-28 23:08_

```suggestion
        // We currently return `Todo` for all async comprehensions,
        // including loops that have invalid syntax
```

---

_@AlexWaygood reviewed on 2024-09-28 23:09_

Nice! This looks pretty good to me. Here's a few nitpicks about comments

---

_@Slyces reviewed on 2024-09-29 08:05_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:241 on 2024-09-29 08:05_

Ah sorry I just noticed that some comments started with a capital and some didn't, maybe didn't entirely pay attention 

---

_Review requested from @AlexWaygood by @Slyces on 2024-09-29 08:41_

---

_@AlexWaygood requested changes on 2024-09-29 14:47_

Thank you! I think there are a few other places where we could use this new variant:

### (1) Both of these branches here:

https://github.com/astral-sh/ruff/blob/bee498d6359dd7ce9aa8bff685ca821ab1bbfe31/crates/red_knot_python_semantic/src/types.rs#L502-L506

Objects that have literal-`int` or literal-`bool` types do have attributes and methods; we shouldn't just infer `Unknown` for all member accesses in these branches. For all attribute lookups in these branches, we'll either need to special-case the specific attribute or defer to the typeshed stubs for `bool` / `int`; `Type::TODO` seems better than `Type::Unknown` to indicate that.

### (2) These two branches here:

https://github.com/astral-sh/ruff/blob/bee498d6359dd7ce9aa8bff685ca821ab1bbfe31/crates/red_knot_python_semantic/src/types.rs#L732-L735

Once we support `type[T]`, we can infer `type[Any]` and `type[Unknown]` for these branches, which is more precise than `Any` and `Unknown`. For example, whereas `<instance_of_Any>.__module__` is `Any`, `<instance_of_type_Any>.__module__` is a `str`. So `Type::TODO` is appropriate here, I think.

### (3) These branches:

https://github.com/astral-sh/ruff/blob/bee498d6359dd7ce9aa8bff685ca821ab1bbfe31/crates/red_knot_python_semantic/src/types/infer.rs#L783-L785

https://github.com/astral-sh/ruff/blob/bee498d6359dd7ce9aa8bff685ca821ab1bbfe31/crates/red_knot_python_semantic/src/types/infer.rs#L1027-L1031

As the comments say, we should be able to infer much more precise types for these once we've implemented some currently-missing features

### (4) The `Unknown` branch here:

https://github.com/astral-sh/ruff/blob/bee498d6359dd7ce9aa8bff685ca821ab1bbfe31/crates/red_knot_python_semantic/src/types/infer.rs#L980-L986

We should be able to infer better types for bindings in situations such as `except (OSError, TypeError) as e` -- we currently infer the type of `e` as `Unknown`, but it should really be `OSError | TypeError`. Similarly for `except EXCEPTIONS as f` where `EXCEPTIONS` is a variable that has type `tuple[type[OSError], type[TypeError]]` -- we'll currently just infer `Unknown` for `f`, but the type of `f` should also be `OSError | TypeError`.

---

There's also one place where I think we should add a comment, because `Unknown` is correct but it's not obvious why it's correct:

```diff
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -968,6 +968,8 @@ impl<'db> TypeInferenceBuilder<'db> {
         let node_ty = except_handler_definition
             .handled_exceptions()
             .map(|ty| self.infer_expression(ty))
+            // If there is no handled exception, it's invalid syntax;
+            // a diagnostic will have already been emitted
             .unwrap_or(Type::Unknown);
 
         let symbol_ty = if except_handler_definition.is_star() {
```

---

_Comment by @Slyces on 2024-09-29 15:44_

Thank you so much for looking into this with so much detail, and sorry for missing some cases. Branches 1, 3, 4 lead to no issues with existing tests.

Branch 2 however requires additional code for invalid syntax in comprehensions as:
- Invalid comprehensions can have `Type::Unknown` as the target iterator
- `Type::Unknown`'s metaclass being `Unknown`, the method `Type::iterate` end up yielding `Unknown`
- When `Type::Unknown`'s metaclass becomes `Type::Todo`, this does not work anymore

I fixed this by introducing an explicit handling of `Unknown` (and we might need one for `Any` as well, but I didn't find a way to reliably create an `Any` var to test it)


---

_Review requested from @AlexWaygood by @Slyces on 2024-09-29 15:44_

---

_@AlexWaygood reviewed on 2024-09-29 15:49_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:514 on 2024-09-29 15:49_

```suggestion
            Type::BooleanLiteral(_) => Type::Todo,
```

---

_Review requested from @AlexWaygood by @Slyces on 2024-09-29 15:56_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1281 on 2024-09-29 16:27_

I think having all three of these would lead to very noisy tracing output. We should be able to easily figure out the inferred type of a `for` loop variable using `reveal_type`, so I'm not sure we really need any of them for debugging issues from users; I think I'd remove all three

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1611 on 2024-09-29 16:27_

not sure we need this in the default tracing output

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2242 on 2024-09-29 16:30_

the comment here is useful, but I think these cases here are fully covered by the branch immediately following this; I don't think you need to add any code to this method. All tests pass if I make this change:

```suggestion
            // When interacting with Todo, Any and Unknown should propagate (as if we fix this
            // `Todo` in the future, the result would then become Any or Unknown, respectively.)
```

---

_@AlexWaygood reviewed on 2024-09-29 16:30_

---

_@Slyces reviewed on 2024-09-29 16:34_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:1611 on 2024-09-29 16:34_

That was temporary code that I missed removing 

---

_@Slyces reviewed on 2024-09-29 16:58_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:1281 on 2024-09-29 16:58_

Sorry! That was also a temporary code I used to isolate the bug - not meant to be commited!

---

_@AlexWaygood reviewed on 2024-09-29 16:59_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:666 on 2024-09-29 16:59_

```suggestion
```

---

_@AlexWaygood approved on 2024-09-29 17:02_

LGTM! I'll wait before merging in case Micha or Carl have any concerns, though.

Possibly we could change the variant so that it is instead `Type::Todo(&'static str)` -- and we could store the `TODO` message in the variant itself, including it as part of the variant's display. That would mean we wouldn't have to have a separate `// TODO` comment for every time where we use this variant. However, I think that that could/should be implemented as a followup, since it adds more complexity to the current code.

---

_Label `red-knot` added by @AlexWaygood on 2024-09-30 11:29_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:254 on 2024-09-30 20:47_

I'd use a simpler wording here. The exact interaction with Any or Unknown depends on the specific case.

```suggestion
    /// output to be unknown. An output should only be `Todo` if fixing all `Todo` inputs to be not `Todo` would change the output type.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:248 on 2024-09-30 20:48_

This is an important clarification:
```suggestion
    /// Temporary type for symbols that can't be inferred yet because of missing implementations. Behaves equivalently to `Any`.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:582 on 2024-09-30 20:49_

This could go up above in the same case with `Any`, `Unknown`, etc.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:636 on 2024-09-30 20:50_

Let's move this up next to the similar `Any` and `Unknown` cases.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:729 on 2024-09-30 20:53_

Again, I'd prefer if we keep `Todo` close to `Any` and `Unknown` cases in matches, since they are closely related.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:228 on 2024-09-30 20:56_

The correct behavior is for Todo to not cancel itself in an intersection, same as Any and Unknown.

I would not add this comment, here or in `add_negative`; rather, we have an existing TODO comment in `add_negative` to stop self-cancelling Any and Unknown; let's just mention Todo in that comment also (and maybe add the same comment here in `add_positive`)

---

_@carljm approved on 2024-09-30 20:58_

This is great, thank you!! I think this will really improve debuggability. Just a few small nits before we merge.

---

_Merged by @carljm on 2024-09-30 21:28_

---

_Closed by @carljm on 2024-09-30 21:28_

---

_Branch deleted on 2024-09-30 21:30_

---
