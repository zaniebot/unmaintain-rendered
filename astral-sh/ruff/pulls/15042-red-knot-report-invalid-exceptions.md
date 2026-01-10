```yaml
number: 15042
title: "[red-knot] Report invalid exceptions"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-exceptions
created_at: 2024-12-18T00:02:19Z
updated_at: 2024-12-18T18:32:49Z
url: https://github.com/astral-sh/ruff/pull/15042
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Report invalid exceptions

---

_Pull request opened by @InSyncWithFoo on 2024-12-18 00:02_

## Summary

Resolves #15038.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2024-12-18 00:02_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2024-12-18 00:02_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2024-12-18 00:02_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2024-12-18 00:02_

---

_Comment by @github-actions[bot] on 2024-12-18 00:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/exception/basic.md`:172 on 2024-12-18 00:38_

Might be nice to have a happy-path test similar to this but showing that we can catch an exception and `raise from` that exception without error. So like this test, but with a valid exception type in place of `int`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:202 on 2024-12-18 00:40_

```suggestion
    /// Checks for `raise` statements and exception handlers
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:731 on 2024-12-18 00:41_

```suggestion
                "Cannot raise object of type `{}` (must be a `BaseException` subclass or instance)",
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:742 on 2024-12-18 00:41_

```suggestion
                "Cannot use object of type `{}` as exception cause (must be a `BaseException` subclass or instance, or `None`)",
```

---

_@carljm reviewed on 2024-12-18 00:44_

Nice! This looks good to me.

I'd like to get @AlexWaygood review on it, since he's done most of the work on exceptions so far. Feel free to hold off on implementing my comments until he reviews also.

---

_@MichaReiser reviewed on 2024-12-18 07:43_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:737 on 2024-12-18 07:43_

```suggestion
    pub(super) fn add_invalid_exception_caught(&mut self, db: &dyn Db, node: &ast::Expr, ty: Type) {
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:232 on 2024-12-18 07:46_

Seeing the changes to the documentation, I now think that these should be two error codes: `invalid-exception-caught` and `invalid-exception-raised` as @AlexWaygood suggested in the linked issue

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1468 on 2024-12-18 07:48_

```suggestion
    pub(crate) fn can_be_raised(self, db: &'db dyn Db) -> bool {
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1474 on 2024-12-18 07:48_

I'm interested in hearing other opinions. These methods feel too specific to me to have them on `Type`. That's why I'm leaning towards making them standalone functions. Unless we forsee using them in other places too (I want to avoid that `Type` ends up with a trillion very specific methods) 

---

_@MichaReiser reviewed on 2024-12-18 07:48_

---

_Label `red-knot` added by @MichaReiser on 2024-12-18 07:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1474 on 2024-12-18 10:49_

I agree with @MichaReiser's comment here. None of these methods are particularly complicated (so separating them out into a separate methods doesn't really improve readability) and they all have very few uses currently. I'm also not sure they're going to be generally useful in many other contexts. Until we have other places where we want to use them, we should just inline them at their uses, in my opinion.

This is also not the way I would write this function. An object can be an exception cause if its type is assignable to the type `BaseException | type[BaseException] | None`. That's _not_ exactly what this function does -- you return `true` if the object is assignable to `BaseException` or is assignable to `type[BaseException]` or actually is (_not_ is assignable to) `None`.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2200 on 2024-12-18 10:52_

Please keep the explicit unpacking of the AST node. It means that we get a compiler error warning about unused variables if we forget to infer the type of one of the sub-nodes. That's very valuable.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2189 on 2024-12-18 11:08_

As you've seen before in your PRs, red-knot panics if we fail to infer the types for all sub-nodes in an AST. The way you've written this method means that if `exc` is `None` but `cause` is `Some()`, red-knot will not infer the type of the `cause` sub-node, and might panic. Now, with valid Python syntax, it's impossible for `cause` to be `Some()` if `exc` is `None` -- but red-knot needs to be able to analyze invalid Python syntax without panicking as well as valid Python syntax. I think currently our parser will never produce an `ast.Raise` node where `exc` is `None` but `cause` is `Some()`, even when analyzing invalid syntax -- but that's an implementation detail of our parser as it is today; we might change the way we do error recovery at any point, so we shouldn't rely on it.

TL;DR: we can't do an early return here; you have to make sure that we also infer the type of `cause` here if it's `Some()`.

---

_@AlexWaygood requested changes on 2024-12-18 11:08_

---

_@AlexWaygood reviewed on 2024-12-18 11:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:232 on 2024-12-18 11:10_

Yeah, I agree that this would be clearer.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:737 on 2024-12-18 16:23_

`add_invalid_exception_cause` is correct here; this error is about the "cause", or the `e` in `raise Foo from e`, it's not about catching exceptions. 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:232 on 2024-12-18 16:24_

Two or three separate codes? On the raising side, there's the raised exception, and there's the cause: the `Foo` and the `e` in `raise Foo from e`. I think if we split it out, we kind of have to go to three, or else one of the two will still have an awkward name: `invalid-exception-raised` doesn't make sense for a bad "cause", because that's not the exception raised.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1474 on 2024-12-18 16:33_

I would like us to increase our use of well-named functions that describe some semantic of a type, rather than just inlining all of it into type inference, particularly when the same logic would be repeated in more than one location. I do think this makes type inference more readable. I think these methods are worth it if they have a sensible/clear name and have 2+ uses. If they have only one use, I agree that's questionable.

As for where they go, I would prefer to keep them on `Type`; I think that's better than a collection of standalone functions which operate on a single `Type`, both in usage ergonomics and in discoverability. I'm not worried about too many methods on `Type`, if we go with the heuristic that methods should generally have 2+ uses, and have a name that clearly represents something semantically meaningful.

Concretely in this case I think those heuristics would suggest keeping `is_exception_class` and `is_exception`, but not `is_raisable` or `can_be_exception_cause`. (I also think `is_none` is fine in general, but as Alex points out, maybe isn't the clearest option here -- though given that `None` is final and that will never change, it really doesn't matter in practice.)

---

_@carljm reviewed on 2024-12-18 16:34_

---

_@AlexWaygood reviewed on 2024-12-18 16:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:232 on 2024-12-18 16:35_

I think it would be fine to have two codes: `invalid-exception-caught` and `invalid-raise-statement`. One code would be about exception handlers, the other would be about `raise` statements.

---

_@carljm reviewed on 2024-12-18 16:40_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:232 on 2024-12-18 16:40_

Sounds reasonable. Maybe just `invalid-raise`? Not sure `-statement` adds value.

---

_@AlexWaygood reviewed on 2024-12-18 16:44_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1474 on 2024-12-18 16:44_

> Concretely in this case I think those heuristics would suggest keeping `is_exception_class` and `is_exception`, but not `is_raisable` or `can_be_exception_cause`.

`Type::is_exception()` is a one-liner, and I honestly find `.is_assignable_to(KnownClass::BaseException.to_instance(db))` more readable, because I don't have to lookup exactly what `is_exception()` is doing under the hood.

`is_exception_class` is a bit more complicated than `is_exception`, because `KnownClass::to_subclass_of()` returns `Option<Type>` rather than `Type`. That's probably a bad signature for `to_subclass_of()` to have, to be honest; I probably made a mistake there when I added it in https://github.com/astral-sh/ruff/commit/ab26d9cf9a5899406e1b4a3555c54e6d0be8ed96#diff-42314c006689490bbdfbeeb973de64046b3e069e3d88f67520aeba375f20e655R1910. Both `KnownClass::to_class_literal()` and `KnownClass::to_instance()` fall back to `Unknown` if they can't find the type being looked up rather than returning an `Option`; for consistency, `KnownClass::to_subclass_of()` should probably return `type[Unknown]` if it can't find the type in question, rather than returning an `Option`. If we fixed the signature of `KnownClass::to_subclass_of()` so that it no longer returns an `Option`, then `is_exception_class()` would also just become a trivial one-liner.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1474 on 2024-12-18 16:50_

Ok, I think it's fair to also add the heuristic "encapsulates some non-trivial logic" :) And I agree that making `to_subclass_of` just handle the fallback to `Unknown` internally is a good alternative to `is_exception_class`.

---

_@carljm reviewed on 2024-12-18 16:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:238 on 2024-12-18 17:29_

```suggestion
    /// Checks for `raise` statements that raise non-exceptions or use invalid
    /// causes for their raised exceptions.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:244 on 2024-12-18 17:31_

```suggestion
    /// Only subclasses or instances of `BaseException` can be raised.
    /// For an exception's cause, the same rules apply, except that `None` is also
    /// permitted. Violating these rules results in a `TypeError` at runtime.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:283 on 2024-12-18 17:37_

```suggestion
    /// ## Examples
    /// ```python
    /// def f():
    ///     try:
    ///         something()
    ///     except NameError:
    ///         raise "oops!" from f
    ///
    /// def g():
    ///     raise NotImplemented from 42
    /// ```
    ///
    /// Use instead:
    /// ```python
    /// def f():
    ///     try:
    ///         something()
    ///     except NameError as e:
    ///         raise RuntimeError("oops!") from e
    ///
    /// def g():
    ///     raise NotImplementedError from None
    /// ```
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1446 on 2024-12-18 17:39_

This method also returns `true` (and is correct to do so) for `Type::Any`, `Type::Unknown` and `Type::Todo`, so if we're going to keep it for now, it would be better to give it a more precise name than `is_exception_class`:

```suggestion
    pub(crate) fn is_assignable_to_subclass_of_base_exception(self, db: &'db dyn Db) -> bool {
```

But I would still favour just simplifying the signature of `KnownClass::to_subclass_of()` so that we don't need this method at all

---

_@AlexWaygood reviewed on 2024-12-18 17:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2322 on 2024-12-18 18:15_

```suggestion
            .unwrap_or(Type::subclass_of_base(ClassBase::Unknown))
```

---

_@AlexWaygood approved on 2024-12-18 18:16_

Nice, thank you!

---

_@AlexWaygood reviewed on 2024-12-18 18:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2205 on 2024-12-18 18:19_

```suggestion

        let can_be_raised =
            UnionType::from_elements(self.db(), [base_exception_type, base_exception_instance]);
        let can_be_exception_cause = UnionType::from_elements(self.db(), [can_be_raised, Type::none(self.db())]);
```

---

_@AlexWaygood reviewed on 2024-12-18 18:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2204 on 2024-12-18 18:26_

```suggestion
        let can_be_exception_cause =
            UnionType::from_elements(self.db(), [can_be_raised, Type::none(self.db())]);
```

---

_Merged by @AlexWaygood on 2024-12-18 18:31_

---

_Closed by @AlexWaygood on 2024-12-18 18:31_

---

_Branch deleted on 2024-12-18 18:31_

---
