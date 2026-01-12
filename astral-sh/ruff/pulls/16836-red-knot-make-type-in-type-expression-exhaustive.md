```yaml
number: 16836
title: "[red-knot] Make' Type::in_type_expression()' exhaustive for Type::KnownInstance"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: make-in-type-expression-exhaustive
created_at: 2025-03-18T18:16:57Z
updated_at: 2025-03-19T15:06:43Z
url: https://github.com/astral-sh/ruff/pull/16836
synced_at: 2026-01-12T15:55:59Z
```

# [red-knot] Make' Type::in_type_expression()' exhaustive for Type::KnownInstance

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

fixes #15048 
We want to handle more types from Type::KnownInstance 

## Test Plan

Add tests for each type added explicitly in the match


---

_Comment by @MatthewMckee4 on 2025-03-18 18:17_

@AlexWaygood what do you think of this to start?

---

_Comment by @github-actions[bot] on 2025-03-18 18:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3272 on 2025-03-18 22:35_

`Protocol` doesn't take arguments; it's never usable directly in an annotation. It should only be inherited. So it probably needs its own `InvalidProtocol` error, it can't fit into the general case of `InvalidBareType`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/protocols.md`:22 on 2025-03-18 22:39_

This error message should just say "`typing.Protocol`" cannot be used in a type expression"

---

_@carljm reviewed on 2025-03-18 22:39_

This looks pretty good! Just `Protocol` needs attention.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3567 on 2025-03-18 22:42_

I would call this `BareSpecialForm` instead. We don't need the `Invalid` prefix here (neither of the other variants have it), and `SpecialForm` more precisely describes the category of thing we are dealing with here than `Type` does.

---

_@carljm reviewed on 2025-03-18 22:42_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types.rs`:3272 on 2025-03-18 22:50_

Sorry yeah I should've caught that

---

_@MatthewMckee4 reviewed on 2025-03-18 22:50_

---

_@MatthewMckee4 reviewed on 2025-03-18 22:52_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types.rs`:3567 on 2025-03-18 22:52_

I did it to match the InvalidType enum lower down. I can change it to that though 

---

_@AlexWaygood reviewed on 2025-03-18 23:05_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/optional.md`:54 on 2025-03-18 23:05_

This should say "exactly one argument"

---

_@MatthewMckee4 reviewed on 2025-03-19 00:14_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/resources/mdtest/annotations/optional.md`:54 on 2025-03-19 00:14_

Should i create different enum types for different classes of errors here, like
- Exactly one argument
- At least one argument

---

_Comment by @MatthewMckee4 on 2025-03-19 00:23_

I'm not fully sure about the names BareSpecialFormAtLeastOne and BareSpecialFormExactlyOne, @AlexWaygood @carljm  what do you think?

---

_Comment by @carljm on 2025-03-19 00:37_

The split you've made looks good. I think the names you've chosen are clear and I wouldn't argue with them, but it would probably also be ok to be a bit less verbose and go with `RequiresOneArgument` and `RequiresArguments`. 

---

_Marked ready for review by @MatthewMckee4 on 2025-03-19 00:40_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-03-19 00:40_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-03-19 00:40_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-03-19 00:41_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-03-19 00:41_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:213 on 2025-03-19 05:10_

I think this should still be present as there are references to it in the document.

---

_Label `red-knot` added by @dhruvmanila on 2025-03-19 05:11_

---

_@dhruvmanila reviewed on 2025-03-19 05:12_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:452 on 2025-03-19 05:14_

I think it might make sense to include these in their respective section as they already contains some invalid form:
* `TypeOf`: https://github.com/MatthewMckee4/ruff/blob/make-in-type-expression-exhaustive/crates/red_knot_python_semantic/resources/mdtest/type_api.md#typeof
* `CallableTypeFromFunction`: https://github.com/MatthewMckee4/ruff/blob/make-in-type-expression-exhaustive/crates/red_knot_python_semantic/resources/mdtest/type_api.md#callabletypefromfunction

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:865 on 2025-03-19 05:16_

I'd suggest to avoid breaking them up unless you need to add some prose in between the two code snippets.

```suggestion
## Invalid

```py
from knot_extensions import Intersection, Not

# error: [invalid-type-form] "`knot_extensions.Intersection` requires at least one argument when used in a type expression"
def f(x: Intersection) -> None:
    reveal_type(x)  # revealed: Unknown


# error: [invalid-type-form] "`knot_extensions.Not` requires exactly one argument when used in a type expression"
def f(x: Not) -> None:
    reveal_type(x)  # revealed: Unknown
```
```

---

_@dhruvmanila reviewed on 2025-03-19 05:20_

---

_Review requested from @dhruvmanila by @MatthewMckee4 on 2025-03-19 11:10_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3583 on 2025-03-19 14:35_

Personally I feel like the `InTypeExpression` part of this is redundant with the enum name `InvalidTypeExpression`; I would just name this variant `Protocol`. But I see there is already precedent with `ClassVarInTypeExpression` and `FinalInTypeExpression`, so I'll go ahead and merge it this way for consistency.

---

_@carljm reviewed on 2025-03-19 14:35_

---

_@carljm approved on 2025-03-19 14:36_

---

_Comment by @carljm on 2025-03-19 14:36_

Thank you for the PR!!

---

_Merged by @carljm on 2025-03-19 14:36_

---

_Closed by @carljm on 2025-03-19 14:36_

---

_@AlexWaygood reviewed on 2025-03-19 14:36_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3307 on 2025-03-19 14:36_

I think the only two of these that are valid in type expressions when they are not parameterized are `TypingSelf` and `TypeAlias`:

- `ReadOnly`, `NotRequired`, `TypeIs`, `TypeGuard`, `Unpack` and `Required` all require exactly one argument
- `Concatenate` requires at least two arguments

---

_@AlexWaygood reviewed on 2025-03-19 14:37_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3307 on 2025-03-19 14:37_

`ReadOnly`, `NotRequired` and `Required` are also type qualifiers, so they are only valid in annotation expressions, not type expressions, even when they do have the necessary number of arguments

---

_@MatthewMckee4 reviewed on 2025-03-19 14:39_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types.rs`:3307 on 2025-03-19 14:39_

arent ReadOnly, NotRequired, TypeIs, TypeGuard, Unpack and Required only allowed in certain places though? Like how ClassVar and Final are invalid here?

---

_@AlexWaygood reviewed on 2025-03-19 14:46_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3307 on 2025-03-19 14:46_

That's correct. But the `KnownInstance` branches of this method are essentially asking the question, "Is this symbol valid in a type expression if it appears without any type arguments?". And the answer is definitely "no" for all of `ReadOnly`, `NotRequired`, `TypeIs`, `TypeGuard`, `Unpack` and `Required`. It's true that these symbols wouldn't even be allowed in a _all_ type expressions even if they _did_ have the correct number of type arguments. But that's not the question that this method is asking ;)

---

_@AlexWaygood reviewed on 2025-03-19 14:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3307 on 2025-03-19 14:58_

In terms of the exact error messages we should emit, I think probably:

- `ReadOnly`, `NotRequired`, `Required` => ``"Type qualifier `{}` is not allowed in type expressions (only in annotation expressions, and only with exactly one argument)"``
- `TypeIs`, `TypeGuard`, `Unpack` => ``"`{}` requires exactly one argument when used in a type expression"``
  - you could reuse your existing `InvalidTypeExpression::RequiresOneArgument` variant for these
- `Concatenate` => ``"`{}` requires at least two arguments when used in a type expression"``
  - you could maybe rework the `InvalidTypeExpression::BareAnnotated` variant so that it can be used to report errors for `Concatenate` as well as `Annotated`

Would you be interested in filing a followup PR? :-)

It could also be nice to have more precise `todo_type!()` messages for the `TypingSelf` and `TypeAlias` variants: ``todo_type!("Support for `typing.Self`")`` and ``todo_type!("Support for `typing.TypeAlias`")``, respectively

---

_@MatthewMckee4 reviewed on 2025-03-19 15:00_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types.rs`:3307 on 2025-03-19 15:00_

Yeah ill get started on that, thanks

---

_@MatthewMckee4 reviewed on 2025-03-19 15:01_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types.rs`:3307 on 2025-03-19 15:01_

> ReadOnly, NotRequired, Required => "Type qualifier `{}` is not allowed in type expressions (only in annotation expressions, and only with exactly one argument)"

Am i right in saying that these should be the same message as FinalInTypeExpression and ClassVarInTypeExpression? 

---

_@AlexWaygood reviewed on 2025-03-19 15:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3307 on 2025-03-19 15:01_

Amazing, thank you! You're doing great work :D

---

_Branch deleted on 2025-03-19 15:01_

---

_@AlexWaygood reviewed on 2025-03-19 15:06_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3307 on 2025-03-19 15:06_

> Am i right in saying that these should be the same message as FinalInTypeExpression and ClassVarInTypeExpression?

Not _quite_. `ClassVar`, `Final`, `Required`, `NotRequired` and `ReadOnly` are all type qualifiers, so they're all valid in annotation expressions but invalid in type expressions. Nonetheless, I think it would be nice to have different error messages because of the fact that they differ in the number of arguments they accept when they appear in annotation expressions:
- `Final` can be used both with and without type arguments in an _annotation_ expression
- `ClassVar` it's... sort-of unclear whether it's valid without type arguments or not (see discussion at https://discuss.python.org/t/bare-classvar-annotation/81705)
- All the rest need exactly one argument when they appear in annotation expressions

So that implies that for the error messages, `Final` and `ClassVar` should have this error message:

```
"Type qualifier `{}` is not allowed in type expressions (only in annotation expressions)"
```

But for the other three, they should have this error message:

```
"Type qualifier `{}` is not allowed in type expressions (only in annotation expressions, and only with exactly one argument)"
```

---
