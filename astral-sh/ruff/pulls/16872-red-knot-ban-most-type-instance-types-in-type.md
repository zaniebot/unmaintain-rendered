```yaml
number: 16872
title: "[red-knot] Ban most `Type::Instance` types in type expressions"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: ban-most-instance-type-in-type-expression
created_at: 2025-03-20T15:00:47Z
updated_at: 2025-03-21T22:00:24Z
url: https://github.com/astral-sh/ruff/pull/16872
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] Ban most `Type::Instance` types in type expressions

---

_Pull request opened by @MatthewMckee4 on 2025-03-20 15:00_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Catch Some Instances, but raise type error for the rest of them
Fixes #16851 

## Test Plan

Extend invalid.md in annotations


---

_Review requested from @carljm by @MatthewMckee4 on 2025-03-20 15:00_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-03-20 15:00_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-03-20 15:00_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-03-20 15:00_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-20 15:02_

---

_@AlexWaygood reviewed on 2025-03-20 15:07_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:1117 on 2025-03-20 15:07_

you can fix the failing `known_class_doesnt_fallback_to_unknown_unexpectedly_on_low_python_version` test by looking up the symbol in the `typing_extensions` module rather than the `typing` module. The `typing` module doesn't have the `ParamSpec` and `TypeVarTuple` classes on low Python versions, but the `typing_extensions` module has it on all versions:

```suggestion
            Self::SpecialForm | Self::TypeVar | Self::StdlibAlias | Self::SupportsIndex => {
                KnownModule::Typing
            }
            Self::TypeAliasType | Self::TypeVar | Self::ParamSpec => KnownModule::TypingExtensions,
```

---

_Comment by @github-actions[bot] on 2025-03-20 15:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
isort (https://github.com/pycqa/isort)
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/isort/isort/wrap.py:16:33: Variable of type `Enum` is not allowed in a type expression
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/isort/isort/wrap_modes.py:12:33: Variable of type `Enum` is not allowed in a type expression
- Found 79 diagnostics
+ Found 81 diagnostics

```
</details>


---

_@MatthewMckee4 reviewed on 2025-03-20 15:08_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/class.rs`:1117 on 2025-03-20 15:08_

Thank you, I should've caught that sorry

---

_Comment by @AlexWaygood on 2025-03-20 15:13_

From the mypy_primer diff, it looks like we'll also need to add special cases to avoid false positives on `NewType` and `GenericAlias`. I think we already have a `KnownClass` variant for `GenericAlias`, but I don't think we have one for `NewType` yet.

---

_@carljm approved on 2025-03-20 16:10_

Nice!

---

_Comment by @carljm on 2025-03-20 16:14_

Oh, reviewed the code without looking at the mypy-primer and tomllib results. We're still seeing a lot of "Variable of type 'object' not allowed" which look like false positives, probably need to get that resolved before landing this.

---

_Closed by @AlexWaygood on 2025-03-20 21:50_

---

_Reopened by @AlexWaygood on 2025-03-20 21:50_

---

_Comment by @carljm on 2025-03-20 22:19_

Looks good to me now! The only new false positive we see now in mypy-primer is with the edge case of creating an enum type imperatively by calling the `enum.Enum` constructor. I think it's fine to leave this false positive in place for now.

---

_Merged by @carljm on 2025-03-20 22:19_

---

_Closed by @carljm on 2025-03-20 22:19_

---

_Comment by @carljm on 2025-03-20 22:20_

Thanks for the PR!

---

_Branch deleted on 2025-03-21 22:00_

---
