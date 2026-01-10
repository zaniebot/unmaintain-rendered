```yaml
number: 14918
title: "[red-knot] Fixup a few edge cases regarding `type[]`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/type-t-edge-cases
created_at: 2024-12-11T14:21:26Z
updated_at: 2024-12-12T16:53:05Z
url: https://github.com/astral-sh/ruff/pull/14918
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Fixup a few edge cases regarding `type[]`

---

_Pull request opened by @AlexWaygood on 2024-12-11 14:21_

## Summary

There's a functional change here, and a few cosmetic changes.

The functional change is that we currently consider `Literal[str]` to be disjoint from `type[Any]`. But they're not disjoint: `Any` here could be materialized as `str` or `object`; in those materializations, the two types would clearly overlap, and therefore `type[Any]` overlaps with `Literal[str]`. This PR fixes our logic in `Type::is_disjoint_from` to reflect that.

Also included in this PR are cosmetic changes to `Type::is_subtype_of`. The main reason for these changes is that currently we have `Type::SubclassOf(target_class))` branches in `match` statements where the `target_class` variable actually has type `ClassBase` rather than `Class`; this is quite confusing. A secondary reason for these changes is that it should be impossible for us to encounter `Type::SubclassOf(SubclassOfType { base })` instances in this `match` statement where `base` is not a `ClassBase::Class()` variant, since all other `ClassBase::Class()` variants are not fully static exit, and therefore we immediately early in `is_subtype_of()` here if we encounter one of them:

https://github.com/astral-sh/ruff/blob/6e11086c981f0ffb179fdaa78fa0837aa0283cda/crates/red_knot_python_semantic/src/types.rs#L607-L612

The changes to `Type::is_subtype_of()` clarify that we shouldn't be dealing with any other `ClassBase::Class()` variants in this `match` statement; we should therefore be using `Class::is_subclass_of()` rather than `Class:is_subclass_of_base()`.

As a result of the above changes, `Class::is_subclass_of_base()` actually becomes unused, so this PR removes it as well. I think this is probably also for the best: a `ClassBase` and a `Class` are two different things, and it feels like it blurs the two concepts to have a method that asks whether a class is a "subclass of" a `ClassBase`. If we need to ask this question again in the future, it's not that much more verbose to just do `class.mro(db).contains(&base)`.

## Test Plan

`cargo test -p red_knot_python_semantic`. The new test case for the `is_not_disjoint_from` test is the only one added in this PR that fails on `main`, but the other added test cases seem useful anyway.

---

_Label `red-knot` added by @AlexWaygood on 2024-12-11 14:21_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-11 14:21_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-11 14:21_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-11 14:21_

---

_Comment by @github-actions[bot] on 2024-12-11 14:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:670 on 2024-12-11 16:52_

Took me a bit to realize that we don't need to handle `SubclassOf(object)` here (like the previous code did), because `Instance(type)` is equivalent to `SubclassOf(object)`, and we check equivalence first in `is_subtype_of`. Might be worth a comment, because (taken on its own) this case now appears incomplete.

TBH I think all these type-algebra methods would be easier to read if every case were commented with a Python-type-syntax example of what the case is checking, but that's out of scope for this PR.


---

_@carljm approved on 2024-12-11 16:54_

Thanks. I think I do prefer always explicitly unpacking to a `Class`, and avoiding methods that blur the line between a `ClassBase` and a `Class`.

---

_@AlexWaygood reviewed on 2024-12-11 17:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:670 on 2024-12-11 17:26_

Yeah... I think on balance the early returns at the top of this function make it more robust, but they definitely (for me, at least) make it a little harder to reason about how the big `match` expression works.

---

_@AlexWaygood reviewed on 2024-12-11 17:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:670 on 2024-12-11 17:54_

Uhhh I actually think this branch is still wrong and no tests fail if I delete it entirely. But I think we do need something here.

`Instance(enum.EnumMeta)` should be a subtype of `type[object]` but not a subtype of `type[type]`. Because instances of `enum.EnumMeta` (example: the class `enum.Enum`) are subclasses of `object` but not subclasses of `type`.

I think I've spent too many hours thinking about `type[]` now because my head's spinning and I can't figure out what logic I need to add to make sure that that works as expected 🙃

---

_Comment by @AlexWaygood on 2024-12-11 20:26_

Okay... I tried to add some more comments to the function... and ended up writing https://github.com/astral-sh/ruff/pull/14924... which I will have to finish tomorrow...

---

_@carljm reviewed on 2024-12-11 20:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:670 on 2024-12-11 20:57_

Yeah, I think this case, as currently written, is wrong: "instances of `type`" is not a subtype of "subclasses of `type`".

> `Instance(enum.EnumMeta)` should be a subtype of `type[object]` but not a subtype of `type[type]`. Because instances of `enum.EnumMeta` (example: the class `enum.Enum`) are subclasses of `object` but not subclasses of `type`.

Yeah, I think the most we can say here is that `Instance(<any subclass of type>)` is a subtype of `SubclassOf(object)`. Which is a generalization from what we have already, which is that `Instance(type)` is equivalent to `SubclassOf(object)`. We probably do need to add a case for this here in `is_subtype_of`.

---

_Comment by @AlexWaygood on 2024-12-11 20:59_

I think tomorrow I'll rip out the `is_subtype_of` parts of this so it only deals with `is_disjoint_from`. And I'll continue work on https://github.com/astral-sh/ruff/pull/14924, which deals with the `is_subtype_of` questions in a more comprehensive way.

---

_Comment by @carljm on 2024-12-11 21:04_

> I think tomorrow I'll rip out the `is_subtype_of` parts of this so it only deals with `is_disjoint_from`. And I'll continue work on https://github.com/astral-sh/ruff/pull/14924, which deals with the `is_subtype_of` questions in a more comprehensive way.

Sounds fine, though I also think it would be fine if this PR at least removes the `is_subtype_of` case that we know is wrong and doesn't fail any tests if removed, since we've established that much here. Just makes the follow up PR on is_subtype_of that much easier to review. 

---

_Comment by @AlexWaygood on 2024-12-11 21:07_

Sure

---

_Renamed from "[red-knot] Fixup a few edge cases regarding `type[Any]`" to "[red-knot] Fixup a few edge cases regarding `type[]`" by @AlexWaygood on 2024-12-12 14:29_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-12 14:32_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3059 on 2024-12-12 15:53_

minor nit, but it doesn't seem necessary to explode this onto multiple lines? Is rustfmt doing that because the trailing comma was added?

---

_@carljm approved on 2024-12-12 16:46_

Looks good!

---

_@AlexWaygood reviewed on 2024-12-12 16:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3059 on 2024-12-12 16:48_

no idea why rustfmt is doing it! I can see if I can get rustfmt to undo it

---

_@AlexWaygood reviewed on 2024-12-12 16:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3059 on 2024-12-12 16:52_

I cannot get rustfmt to undo it. Ours not to reason why?

---

_Merged by @AlexWaygood on 2024-12-12 16:53_

---

_Closed by @AlexWaygood on 2024-12-12 16:53_

---

_Branch deleted on 2024-12-12 16:53_

---
