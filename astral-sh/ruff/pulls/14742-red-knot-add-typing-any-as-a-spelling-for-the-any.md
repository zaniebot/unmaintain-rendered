```yaml
number: 14742
title: "[red-knot] Add `typing.Any` as a spelling for the Any type"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/typing-any
created_at: 2024-12-02T19:50:23Z
updated_at: 2024-12-04T14:56:38Z
url: https://github.com/astral-sh/ruff/pull/14742
synced_at: 2026-01-12T15:55:48Z
```

# [red-knot] Add `typing.Any` as a spelling for the Any type

---

_@dcreager_

We already had a representation for the Any type, which we would use e.g. for expressions without type annotations.  We now recognize `typing.Any` as a way to refer to this type explicitly.  Like other special forms, this is tracked correctly through aliasing, and isn't confused with local definitions that happen to have the same name.

Closes #14544 


---

_@dcreager reviewed on 2024-12-02 19:52_

---

_Review comment by @dcreager on `crates/red_knot_vendored/vendor/typeshed/stdlib/typing.pyi`:200 on 2024-12-02 19:52_

I needed this to get the `try_from_module_and_symbol` logic to kick in and handle the `typing.Any` reference as a special case.  I'm assuming this isn't kosher, though, since it's a direct edit to a vendored file.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/any.md`:70 on 2024-12-02 20:37_

Another useful way to verify that we don't have the actual `Any` type here is to try to assign some other type to the name (e.g. `x = "foo"` as in the above tests) and verify that that does emit an invalid-assignment diagnostic.

---

_Comment by @github-actions[bot] on 2024-12-02 20:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1959 on 2024-12-02 20:38_

When we adjust this PR to use unmodified typeshed, this will instead return `KnownClass::Object`

---

_Review comment by @carljm on `crates/red_knot_vendored/vendor/typeshed/stdlib/typing.pyi`:200 on 2024-12-02 20:39_

Yeah, we'll have to instead add the logic for detecting `Any` into `infer_assignment_definition`, without the "annotated as `_SpecialForm`" condition that is checked in `infer_annotated_assignment_definition`.

---

_@carljm reviewed on 2024-12-02 20:39_

---

_Marked ready for review by @dcreager on 2024-12-02 21:11_

---

_Review requested from @MichaReiser by @dcreager on 2024-12-02 21:11_

---

_Review requested from @AlexWaygood by @dcreager on 2024-12-02 21:11_

---

_Review requested from @sharkdp by @dcreager on 2024-12-02 21:11_

---

_@dcreager reviewed on 2024-12-02 21:13_

---

_Review comment by @dcreager on `crates/red_knot_vendored/vendor/typeshed/stdlib/typing.pyi`:200 on 2024-12-02 21:13_

My first stab at this checked whether the file containing the definition was `vendored://stdlib/typing.pyi`.  I ended up switching to something more like the existing `_SpecialForm` detection, which uses `file_to_module` and `try_from_module_and_symbol`.

---

_Label `red-knot` added by @AlexWaygood on 2024-12-02 21:14_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1959 on 2024-12-02 22:25_

I would be inclined to leave out this comment, since a) it's not the only KnownInstance which isn't defined as `_SpecialForm`, b) the fact that it isn't an instance of `_SpecialForm` is self-evident here, and c) it's unlikely to be changed to be an instance of `_SpecialForm`.
```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1980 on 2024-12-02 22:28_

While `Any` is available in `typing_extensions`, it is a simple unconditional re-export from `typing`; there is no Python version in which it is actually defined in `typing_extensions` (unlike the other symbols here.) (This is because it is the oldest of these special forms, and has always existed in the `typing` module.) So we don't ever need to recognize it from a _definition_ in `typing_extensions`; if someone imports it from `typing_extensions`, they're still getting the type from `typing` module.
```suggestion
            ("typing", "Any") => Some(Self::Any),
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1679 on 2024-12-02 22:31_

I'd also be inclined to leave out this comment, since I think typeshed is unlikely to change in this area (there's no strong motivation for the change, and it could cause issues for other type checkers). If typeshed were to change, our tests would alert us to the problem.
```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/mro.rs`:380 on 2024-12-02 22:34_

It's under-specified, and perhaps unfortunate, but the typing spec does actually permit inheriting from `Any`: https://typing.readthedocs.io/en/latest/spec/special-types.html#any

And this is used in typeshed (e.g. for `unittest.mock.Mock`): https://github.com/python/typeshed/blob/main/stdlib/unittest/mock.pyi#L109

This is already supported by the `ClassBase` enum: we should return `Some(Self::Any)` instead of `None` for `KnownInstanceType::Any`. 

---

_@carljm reviewed on 2024-12-02 22:37_

This is excellent, thank you!!

A few suggested changes on non-obvious subtleties.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:1959 on 2024-12-03 14:01_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:1679 on 2024-12-03 14:02_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:1980 on 2024-12-03 14:02_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/mro.rs`:380 on 2024-12-03 19:57_

I've added some tests for subclasses; see below

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/annotations/any.md`:67 on 2024-12-03 19:58_

Is this what we want the revealed class to be?  Or maybe `Subclass | Any` instead?

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/annotations/any.md`:62 on 2024-12-03 20:00_

I think I've updated the inference logic correctly to treat instances of `Subclass` as if they were `Any` instead of `Instance(Subclass)`.  These lines both got assignment errors before the change.

---

_@dcreager reviewed on 2024-12-03 20:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/any.md`:52 on 2024-12-03 20:23_

Sorry, I should have been clearer in my comment about the scope of this PR. I was only intending that we'd return the right `ClassBase` variant when building the MRO, and leave any other updates to the behavior of inheriting-Any as a separate project.

I think the most basic test we can add for this here would just be a test that `Subclass.__mro__` includes `Any/Unknown` when it inherits `Any` explicitly, similar to existing tests in `mro.md`. (That test could live either here or in `mro.md`.)

I've commented below on the semantics that we will want for inheriting-Any, but that doesn't mean I think we should implement those in this PR. Since you've already written some tests, I think it would be reasonable (rather than deleting them) to just update the tests to reflect the current behavior, add TODO comments to reflect the future desired semantics, and leave it like that for now.

If you're interested in implementing more of this behavior as a follow-up PR, that's great! But I'd like to get your first PR landed sooner :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/any.md`:67 on 2024-12-03 20:29_

I think it should be just `Subclass` -- the fact that `Subclass` has `Any` in its MRO is an aspect of the type, but doesn't need to be part of its display.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/any.md`:62 on 2024-12-03 20:33_

I think this comment, and the line `x: Subclass = 1`, are not quite right; that latter assignment should be an error. `Subclass` is not equivalent to `Any`; it's a distinct class, which has an unknown base class. There is no "materialization" (using the term from the typing spec, to mean "replacing Any with a concrete static type") of the unknown base class that could possibly result in `1` being an instance of `Subclass`.

The second test line here is correct; since `Subclass` inherits from an unknown type, that unknown type _could_ be (could materialize to) `int` or a subclass of `int`, therefore we should allow the assignment `y: int = Subclass()`.

You can see that both [pyright](https://pyright-play.net/?enableExperimentalFeatures=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoCCKcAUCQMYA2AhgM61QDKArgEYAURcAlAFwlRBUBHVpkAHryZsoAXigBGEnCmp88lh25kgA) and [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=63d4f40219fb0072ebd811c6ae7937db) agree with this, via their online playgrounds.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/any.md`:62 on 2024-12-03 20:38_

I don't think we want to make that change; `Subclass` should still have the type `Instance(Subclass)`, not the type `Any`. Our representation of the class `Subclass` will have `Any` in its method resolution order (MRO), which will impact how we treat it in some cases. For example, it should be assignable to any other non-final type, since it might be a subclass of that type, as mentioned below, and accessing an unknown attribute on it should give an attribute type of `Any` (the unknown base type might have that attribute, with some unknown type). But these are cases that we will implement in e.g. `Type::is_assignable_to` and `Type::member`, not by making the subclass of Any equivalent to Any.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1366 on 2024-12-03 20:44_

It's awesome that you found the right spot for this and figured out how to do it, but (as described above) I don't think this is quite the behavior we'll want.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1535 on 2024-12-03 20:45_

Similarly here, I don't think this is the behavior we want -- an instance of a class that inherits `Any` should still just be an `Instance` type of that class.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2667 on 2024-12-03 20:46_

This seems likely useful in a future PR when we do want to implement some of the special handling for classes that inherit `Any`, I'd leave it!

---

_@carljm reviewed on 2024-12-03 20:48_

This is quite impressive for your first PR! Nice work.

Unfortunately I wasn't clear about the semantics of inheriting Any (because I wasn't actually expecting you to go ahead and implement those semantics in this PR!), so I think we'll need to revert a couple of the changes here. I would suggest leaving the semantics of inheriting Any to a separate PR.

---

_@carljm reviewed on 2024-12-03 20:51_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/any.md`:62 on 2024-12-03 20:51_

Just for future reference, not because it's relevant to this PR: one place where I think mypy and pyright get it wrong is that they also allow the assignment `x: Fin = Sub()` in [this code sample](https://pyright-play.net/?enableExperimentalFeatures=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoCCKcANFMKgIYA2AULQMbWUDOLUAygK4BGAFETgBKAFy0oEqAlYt6AAQooajZmygAxVGMlSZ9AB4jOvKAF4oARlpwjqfOe78htAF5HNKM8af1aQA). Because `Fin` is a final class, it can have no subclasses, therefore the unknown base class of `Sub` can't materialize to `Fin`, therefore I don't think that assignment should be allowed. But this is for future: we don't even have support for final classes in red-knot at all yet.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:1366 on 2024-12-03 21:29_

Reverted

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:1535 on 2024-12-03 21:29_

Reverted

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/annotations/any.md`:52 on 2024-12-03 21:36_

> I was only intending that we'd return the right `ClassBase` variant when building the MRO, and leave any other updates to the behavior of inheriting-Any as a separate project.

Ha, whoops!

> I think the most basic test we can add for this here would just be a test that `Subclass.__mro__` includes `Any/Unknown` when it inherits `Any` explicitly, similar to existing tests in `mro.md`. (That test could live either here or in `mro.md`.)

Done

> I think it would be reasonable (rather than deleting them) to just update the tests to reflect the current behavior, add TODO comments to reflect the future desired semantics, and leave it like that for now.

Done

> I've commented below on the semantics that we will want for inheriting-Any

Ahh, I misunderstood the intented use case in the spec — thought it was introducing a special case where the subclass should be treated exactly like `Any`.  Your explanation makes more sense!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/annotations/any.md`:67 on 2024-12-03 21:37_

Done

---

_@dcreager reviewed on 2024-12-03 21:38_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/any.md`:52 on 2024-12-03 22:05_

```suggestion
The spec allows you to define subclasses of `Any`.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/any.md`:67 on 2024-12-03 22:09_

The more-detailed comment above is great, but we usually use this format for TODOs in mdtests, right next to the specific change that should happen, just so it's extra-clear to future-us updating the test.

```suggestion
TODO: no diagnostic
x: Subclass = 1  # error: [invalid-assignment]
```

---

_@carljm approved on 2024-12-03 22:11_

Looks excellent! Two very small wording nits. Feel free to merge, and congrats on landing your first red-knot PR!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/annotations/any.md`:67 on 2024-12-04 14:48_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/annotations/any.md`:52 on 2024-12-04 14:48_

Done

---

_@dcreager reviewed on 2024-12-04 14:48_

---

_Merged by @dcreager on 2024-12-04 14:56_

---

_Closed by @dcreager on 2024-12-04 14:56_

---

_Branch deleted on 2024-12-04 14:56_

---
