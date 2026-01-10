```yaml
number: 16850
title: Show more precise messages in invalid type expressions
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: in-type-expression-more-precise-messages
created_at: 2025-03-19T15:26:56Z
updated_at: 2025-03-19T23:45:23Z
url: https://github.com/astral-sh/ruff/pull/16850
synced_at: 2026-01-10T19:40:36Z
```

# Show more precise messages in invalid type expressions

---

_Pull request opened by @MatthewMckee4 on 2025-03-19 15:26_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Some error messages are not very specific and could be improved

## Test Plan

Not yet sure where to put some tests, but will just test invalid type expressions


---

_Label `red-knot` added by @AlexWaygood on 2025-03-19 15:28_

---

_Comment by @MatthewMckee4 on 2025-03-19 15:29_

Im not fully sure where to put tests for the currently unsupported special forms and type qualifiers

---

_Comment by @github-actions[bot] on 2025-03-19 15:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-03-19 15:46_

> Im not fully sure where to put tests for the currently unsupported special forms and type qualifiers

maybe these files?
- https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/resources/mdtest/annotations/unsupported_type_qualifiers.md
- https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/resources/mdtest/annotations/unsupported_special_forms.md

---

_Comment by @MatthewMckee4 on 2025-03-19 15:47_

Yeah, i thought that, but we does providing error messages not imply that we are supporting them partially?

---

_Comment by @AlexWaygood on 2025-03-19 15:49_

True. We'll need comprehensive test suites covering each special form eventually, so you can create new files for each now if you like! But equally, I don't think it matters too much right now; I'd be inclined to just adjust the prose a little bit in those files saying that we don't fully support them, but we do detect some invalid uses when they appear in type expressions.

---

_Comment by @MatthewMckee4 on 2025-03-19 15:50_

Okay, will do that then, thanks

---

_Comment by @MatthewMckee4 on 2025-03-19 16:08_

Tests failing because
```python
def _(
    a: Final
) -> None: ...
```
raises no error

Ill change it to Final | int.

@AlexWaygood why does this happen exactly?

---

_Comment by @AlexWaygood on 2025-03-19 16:14_

> @AlexWaygood why does this happen exactly?

That's because we currently only validate that parameter annotations are valid annotation expressions. Bare `Final` _is_ a valid _annotation expression_, even though it's not a valid _type expression_.

You can take a look at the typing spec page here for an explanation of the difference between the two concepts: https://typing.python.org/en/latest/spec/annotations.html#type-and-annotation-expressions. `Final` is a valid annotation expression but not a valid type expression; `Final | int` is neither a valid annotation expression nor a valid type expression.

Now, is `Final` a valid parameter annotation? No! 😄 But the reason it's invalid isn't relevant to the changes you're making here, and we shouldn't attempt to detect the invalidity as part of the `Type::in_type_expression()` method. At some point, we'll need to add some extra validation specifically for parameter annotations that checks exactly whether the annotation expression is one of the expressions that are disallowed in the specific context of parameter annotations. But we haven't implemented that additional check yet :-)

---

_Comment by @github-actions[bot] on 2025-03-19 16:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MatthewMckee4 on 2025-03-19 16:19_

Okay understood. What function infers the type of parameter annotations?

---

_Marked ready for review by @MatthewMckee4 on 2025-03-19 16:19_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-03-19 16:19_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-03-19 16:19_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-03-19 16:19_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-03-19 16:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/unsupported_special_forms.md`:43 on 2025-03-19 16:27_

```suggestion
One thing that is supported is error messages for using special forms in type expressions.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/unsupported_special_forms.md`:46 on 2025-03-19 16:27_

```suggestion
from typing_extensions import Unpack, TypeGuard, TypeIs, Concatenate
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3611 on 2025-03-19 16:31_

```suggestion
    /// Type qualifiers are always invalid in *type expressions*,
    /// but these ones are okay with 0 arguments in *annotation expressions*
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3613 on 2025-03-19 16:32_

```suggestion
    /// Type qualifiers that are invalid in type expressions,
    /// and which would require exactly one argument even if they appeared in an annotation expression
```

---

_Comment by @carljm on 2025-03-19 16:33_

> What function infers the type of parameter annotations?

`TypeInferenceBuilder::infer_parameter` and `TypeInferenceBuilder::infer_parameter_with_default` both call into `TypeInferenceBuilder::infer_optional_annotation_expression` to infer the annotated type.

---

_@AlexWaygood reviewed on 2025-03-19 16:33_

Looks great, thank you!

---

_@carljm reviewed on 2025-03-19 16:35_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3610 on 2025-03-19 16:35_

Since this PR is removing `ClassVarInTypeExpression` and `FinalInTypeExpression`, can we also rename `ProtocolInTypeExpression` to just `Protocol`?

---

_@MatthewMckee4 reviewed on 2025-03-19 16:36_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types.rs`:3610 on 2025-03-19 16:36_

Yeah

---

_@AlexWaygood approved on 2025-03-19 16:57_

Thanks, this is great!! I pushed a commit to make the `match` a bit more readable, since it was getting a bit big and the `KnownInstance` branches were interspersed with some of the other branches

---

_Merged by @AlexWaygood on 2025-03-19 17:00_

---

_Closed by @AlexWaygood on 2025-03-19 17:00_

---

_Branch deleted on 2025-03-19 17:29_

---
