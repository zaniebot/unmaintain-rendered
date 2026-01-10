```yaml
number: 13819
title: "[red-knot] Enhancing Diagnostics for Compare Expression Inference"
type: pull_request
state: merged
author: cake-monotone
labels:
  - ty
assignees: []
merged: true
base: main
head: refactor-infer-binary-comparison-to-fix-diagnotics
created_at: 2024-10-19T12:57:36Z
updated_at: 2024-10-24T14:58:51Z
url: https://github.com/astral-sh/ruff/pull/13819
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] Enhancing Diagnostics for Compare Expression Inference

---

_Pull request opened by @cake-monotone on 2024-10-19 12:57_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

- Refactored comparison type inference functions in `infer.rs`: Changed the return type from `Option` to `Result` to lay the groundwork for providing more detailed diagnostics.
- Updated diagnostic messages.

This is a small step toward improving diagnostics in the future.

Please refer to #13787

## Test Plan

mdtest included!


---

_Review requested from @carljm by @cake-monotone on 2024-10-19 12:57_

---

_Review requested from @MichaReiser by @cake-monotone on 2024-10-19 12:57_

---

_Review requested from @AlexWaygood by @cake-monotone on 2024-10-19 12:57_

---

_Comment by @github-actions[bot] on 2024-10-19 13:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3473 on 2024-10-19 13:23_

nit:

```suggestion
    const fn dunder_name(self) -> &'static str {
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3588 on 2024-10-19 13:24_

```suggestion
            .ok_or_else(|| OperatorUnsupportedError {
```

---

_@AlexWaygood reviewed on 2024-10-19 13:26_

Thank you! Not at all a full review, just a couple of nits I spotted while skimming

---

_@cake-monotone reviewed on 2024-10-19 14:54_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/infer.rs`:3588 on 2024-10-19 14:54_

Thanks for catching those! 👍 

---

_Review requested from @AlexWaygood by @cake-monotone on 2024-10-19 14:55_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3486 on 2024-10-19 17:57_

minor nit: perhaps this should be called `BinaryOperatorUnsupportedError`, since there are also unsupported unary operations, but this error struct is only suitable for carrying information about a binary operator.

---

_@carljm approved on 2024-10-19 18:00_

Looks great! I just have one minor naming comment; I'll push that and then merge.

---

_@carljm reviewed on 2024-10-19 18:02_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3486 on 2024-10-19 18:02_

Sorry, should be `CompareOperatorUnsupportedError`, since this is actually specific to `ast.CmpOp`. Or maybe for brevity could just be `CompareUnsupportedError`.

---

_Merged by @carljm on 2024-10-19 18:17_

---

_Closed by @carljm on 2024-10-19 18:17_

---

_@cake-monotone reviewed on 2024-10-20 08:07_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/infer.rs`:3486 on 2024-10-20 08:07_

Actually, it’s coming from the rule name
( https://github.com/astral-sh/ruff/pull/13819/files#diff-65c2c229c88f4021638c996a7496384000d9e7b53b08426b34e92f120bd30b06R2783)
But you're right, CompareUnsupportedError makes more sense! I think the rule name could be updated as well, but it’s a pretty minor thing.

---

_@carljm reviewed on 2024-10-21 17:32_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3486 on 2024-10-21 17:32_

We could update the rule name, but it's possible we want to share a rule code for all of these (binary, unary, compare operators). Not sure yet, there will be future work on rule categorization.

---

_Label `red-knot` added by @dhruvmanila on 2024-10-24 14:58_

---
