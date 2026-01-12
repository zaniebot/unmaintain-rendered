```yaml
number: 14397
title: "Understand `typing.Optional` in annotations"
type: pull_request
state: merged
author: Glyphack
labels:
  - ty
assignees: []
merged: true
base: main
head: redknot-typing-optional
created_at: 2024-11-17T09:55:27Z
updated_at: 2024-11-17T17:10:23Z
url: https://github.com/astral-sh/ruff/pull/14397
synced_at: 2026-01-12T15:55:47Z
```

# Understand `typing.Optional` in annotations

---

_@Glyphack_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->
Fix: #14354

## Summary

Recognize `Optional` special form in annotation. Both `typing` and `typing_extensions` modules are valid for importing `Optional`.
The result type is a Union with `None`.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Added tests for importing from modules and assignability and different combinations for the `Optional` parameter.

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-11-17 10:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @MichaReiser on 2024-11-17 10:20_

---

_@Glyphack reviewed on 2024-11-17 10:33_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types.rs`:1810 on 2024-11-17 10:33_

Should this comment be repeated on every special form? Most of them have this properties.

---

_Marked ready for review by @Glyphack on 2024-11-17 10:34_

---

_Review requested from @carljm by @Glyphack on 2024-11-17 10:34_

---

_Review requested from @MichaReiser by @Glyphack on 2024-11-17 10:34_

---

_Review requested from @AlexWaygood by @Glyphack on 2024-11-17 10:34_

---

_Review requested from @sharkdp by @Glyphack on 2024-11-17 10:34_

---

_@MichaReiser reviewed on 2024-11-17 10:51_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1830 on 2024-11-17 10:51_

```suggestion
            Self::Literal | Self::Optional | Self::TypeVar(_) => Truthiness::AlwaysTrue,
```

---

_Renamed from "Understand typing.Optional in annotations" to "Understand `typing.Optional` in annotations" by @Glyphack on 2024-11-17 11:16_

---

_@dhruvmanila reviewed on 2024-11-17 14:15_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:4533 on 2024-11-17 14:15_

I think `Optional` contains type expressions as per the grammar:
```
| <Optional> '[' type_expression ']'
```
So, you'll need to use `self.infer_type_expression` instead.

Ref: https://typing.readthedocs.io/en/latest/spec/annotations.html#grammar-token-expression-grammar-type_expression

---

_@AlexWaygood reviewed on 2024-11-17 15:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1810 on 2024-11-17 15:23_

I find the doc-comments helpful. It's true that many will likely be quite similar, but the comment for an enum variant shows up in the tooltip when you hover over that enum variant in your IDE with the ruff repo open

---

_@Glyphack reviewed on 2024-11-17 15:39_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:4533 on 2024-11-17 15:39_

Thanks I was not aware of that function.

---

_@AlexWaygood approved on 2024-11-17 17:04_

Looks great, thank you!

---

_Merged by @AlexWaygood on 2024-11-17 17:04_

---

_Closed by @AlexWaygood on 2024-11-17 17:04_

---

_Branch deleted on 2024-11-17 17:05_

---
