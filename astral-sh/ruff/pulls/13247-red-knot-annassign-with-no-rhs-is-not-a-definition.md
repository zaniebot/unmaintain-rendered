```yaml
number: 13247
title: "[red-knot] AnnAssign with no RHS is not a Definition"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/annassign-no-rhs
created_at: 2024-09-04T19:27:13Z
updated_at: 2024-09-05T15:55:02Z
url: https://github.com/astral-sh/ruff/pull/13247
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] AnnAssign with no RHS is not a Definition

---

_Pull request opened by @carljm on 2024-09-04 19:27_

My plan for handling declared types is to introduce a `Declaration` in addition to `Definition`. A `Declaration` is an annotation of a name with a type; a `Definition` is an actual runtime assignment of a value to a name. A few things (an annotated function parameter, an annotated-assignment with an RHS) are both a `Definition` and a `Declaration`.

This more cleanly separates type inference (only cares about `Definition`) from declared types (only impacted by a `Declaration`), and I think it will work out better than trying to squeeze everything into `Definition`. One of the tests in this PR (`annotation_only_assignment_transparent_to_local_inference`) demonstrates one reason why. The statement `x: int` should have no effect on local inference of the type of `x`; whatever the locally inferred type of `x` was before `x: int` should still be the inferred type after `x: int`. This is actually quite hard to do if `x: int` is considered a `Definition`, because a core assumption of the use-def map is that a `Definition` replaces the previous value. To achieve this would require some hackery to effectively treat `x: int` sort of as if it were `x: int = x`, but it's not really even equivalent to that, so this approach gets quite ugly.

As a first step in this plan, this PR stops treating AnnAssign with no RHS as a `Definition`, which fixes behavior in a couple added tests.

This actually makes things temporarily worse for the ellipsis-type test, since it is defined in typeshed only using annotated assignments with no RHS. This will be fixed properly by the upcoming addition of declarations, which should also treat a declared type as sufficient to import a name, at least from a stub.

---

_Label `red-knot` added by @carljm on 2024-09-04 19:27_

---

_Review requested from @MichaReiser by @carljm on 2024-09-04 19:27_

---

_Review requested from @AlexWaygood by @carljm on 2024-09-04 19:27_

---

_Comment by @github-actions[bot] on 2024-09-04 19:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:650 on 2024-09-05 05:58_

Nit: I would probably use a match here and combine it with the `if matches!(...)` above

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:653 on 2024-09-05 05:59_

It's a bit unfortunate that we first add the flags and then take them away right after but I don't have a suggestion on how to handle this more elegantly

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3988 on 2024-09-05 06:08_

I think this should be an error because the annotation is incorrect. The annotation should be `x: int | None` because it isn't guaranteed to be initialized in all paths.

---

_@MichaReiser reviewed on 2024-09-05 06:10_

> My plan for handling declared types is to introduce a Declaration in addition to Definition. A Declaration is an annotation of a name with a type; a Definition is an actual runtime assignment of a value to a name. A few things (an annotated function parameter, an annotated-assignment with an RHS) are both a Definition and a Declaration.

I'm a bit concerned about creating two ingredients because of the performance implications. We sofar tried to come up to reduce the ingredients but this approach would increase the number of ingredients. Unfortunately, I don't feel like i have a good enough understanding of what you're trying to do or what motivates to have two ingredients to make a concrete suggestion.


Following the outlined logic, a fully annotated class or function should then also create a `Definition` and a `Declaration` because they are both. Arguably, even a partially annotated function is both (and so are its parameters). 



---

_@AlexWaygood reviewed on 2024-09-05 07:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3988 on 2024-09-05 07:08_

`int | None` would be much less accurate here. `int | None` means "the value could be an instance of `int`, or it could be the `None` singleton". But in no path is `x` assigned to the `None` singleton here, so it's impossible for the value of `x` to ever be `None`.

There's no way in Python to express with type annotations the idea that "this variable may be undefined". We use `Type::Unbound` internally to track whether a variable has been defined in all branches or not, but that's not a specified type in Python's typing system that users are able to utilise in their annotations

---

_@MichaReiser reviewed on 2024-09-05 07:10_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3988 on 2024-09-05 07:10_

Hmm, that's a rather interesting choice in python's type system.

---

_@AlexWaygood reviewed on 2024-09-05 07:15_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3988 on 2024-09-05 07:15_

There have been proposals to improve this, but they've met with a somewhat cool reception. Some people have argued that allowing users to annotate variables as being only possibly defined would encourage users to write more unsafe code. https://mail.python.org/archives/list/typing-sig@python.org/thread/K3ME5IZIV3Q4PTH4PK7YHYJFGBZOLOJK/#K3ME5IZIV3Q4PTH4PK7YHYJFGBZOLOJK

---

_@AlexWaygood reviewed on 2024-09-05 10:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2913 on 2024-09-05 10:33_

Arguably this is actually an improvement, since at least I understand how we arrive at this conclusion now 😆 `Literal[EllipsisType]` is pretty baffling; I don't understand why we think `Ellipsis` is equal to the `EllipsisType` class object

---

_@AlexWaygood reviewed on 2024-09-05 10:38_

I think this is correct in terms of the semantics when it comes to runtime files, but I'd like to hear more about how you plan to handle the semantics of stub files. It seems like there are two possible ways of handling stubs:

1. Say that annotations _do_ actually create definitions in stub files as well as declarations
2. Say that annotations never create definitions, only declarations, but that a declaration has a different meaning in the context of a stub file

It sounds like you've chosen option (2) here but intuitively to me that feels like the more complicated option

---

_@carljm reviewed on 2024-09-05 14:38_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2913 on 2024-09-05 14:38_

It's because of the wrong code in `infer_annotated_assignment` that I removed in this PR, where we inferred the annotation as a value expression and assign that type to the name.

---

_@MichaReiser approved on 2024-09-05 14:41_

There's no reason to block this PR. I think the code here looks good but I'm interested to learn more about how you plan to introduce a new `Declaration` ingredient.

---

_@AlexWaygood reviewed on 2024-09-05 14:53_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4010 on 2024-09-05 14:53_

I guess a question I have here is: so what's the point of this annotation, if our more precise inference takes priority over the declared type? Should we emit a diagnostic warning the user that the declaration will have no effect?

Hmm, and what about something like this? Our precise inference will probably cause the type to be inferred as `list[Literal[1, 2, 3]]`, but that's clearly not what the user wants. And if we infer the precise type, we'll emit a spurious error on the `takes_list_of_ints(x)` call due to list invariance:

```py
x: list[int] = [1, 2, 3]

def takes_list_of_ints(arg: list[int]) -> None:
    pass

takes_list_of_ints(x)
```

---

_@AlexWaygood reviewed on 2024-09-05 14:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2913 on 2024-09-05 14:54_

Ah I see... thanks!

---

_@carljm reviewed on 2024-09-05 14:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:653 on 2024-09-05 14:58_

I think I found a way to address both this and your above comment together; let me know what you think!

---

_@carljm reviewed on 2024-09-05 15:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3988 on 2024-09-05 15:00_

I think I tend to agree with Eric on this one; explicit annotations for undefined would just lead to a bunch more ugly code using `hasattr`, since that's how you'd have to narrow away the maybe-undefinedness. It's better to just have a diagnostic on use of a maybe-undefined name, and on failure to consistently initialize instance attributes.

---

_Comment by @carljm on 2024-09-05 15:05_

It looks like we need more of a design document on declared types implementation; let's move the broader discussion on my PR description there instead, since it's only tangentially related to this PR. This PR makes sense as an initial step regardless, because the added tests are needed and fix current bugs, and the implementation changes will be easy enough to adjust to whatever we do for declared types.

---

_@AlexWaygood approved on 2024-09-05 15:31_

---

_@carljm reviewed on 2024-09-05 15:54_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4010 on 2024-09-05 15:54_

This is something I can try to clarify more in the design doc, but I'll try to answer here briefly.

The `x: int` annotation in this particular code sample has no effect, because there is no assignment to `x` anywhere following it. But in general, the effect that it would have would be that any assignment to `x` after `x: int` must assign something that is assignable to `int`, or else there will be an invalid-assignment diagnostic. Without the `x: int`, any subsequent assignment to `x` would be allowed and we'd just happily allow the shadowing to a new inferred type (or infer the union if it's conditional.)

Regarding generics, let me get into that more in the design document.

---

_Merged by @carljm on 2024-09-05 15:55_

---

_Closed by @carljm on 2024-09-05 15:55_

---

_Branch deleted on 2024-09-05 15:55_

---
