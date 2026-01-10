```yaml
number: 14268
title: "[`flake8-pyi`] Mark fix as unsafe when type annotation contains comments for `duplicate-literal-member` (`PYI062`)"
type: pull_request
state: merged
author: sbrugman
labels:
  - rule
  - fixes
assignees: []
merged: true
base: main
head: pyi062-applicability-comments
created_at: 2024-11-11T11:44:47Z
updated_at: 2024-11-11T12:51:05Z
url: https://github.com/astral-sh/ruff/pull/14268
synced_at: 2026-01-10T20:50:57Z
```

# [`flake8-pyi`] Mark fix as unsafe when type annotation contains comments for `duplicate-literal-member` (`PYI062`)

---

_Pull request opened by @sbrugman on 2024-11-11 11:44_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Correctly mark fix as unsafe when comments are present within the annotation.

## Test Plan

Added a test case

---

_Review requested from @AlexWaygood by @sbrugman on 2024-11-11 11:44_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_literal_member.rs`:80 on 2024-11-11 11:54_

Along the same lines, you could do something like this to further reduce indentation:

```rs
    let Expr::Subscript(subscript) = expr else {
        return;
    };

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_literal_member.rs`:106 on 2024-11-11 11:56_

Thanks. While I've never seen comments inside a `Literal` annotation in "real life", this does seem to align with the rough consensus in https://github.com/astral-sh/ruff/issues/9790. So this change seems good to me.

---

_@AlexWaygood reviewed on 2024-11-11 11:56_

Thanks!

---

_Comment by @github-actions[bot] on 2024-11-11 11:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sbrugman reviewed on 2024-11-11 12:08_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_literal_member.rs`:106 on 2024-11-11 12:08_

I agree it's very rare. My primary motivation for this type of changes is to give the right example for future uses. While developing new rules we often take existing rules as base, and being rigorous now might prevent issues in future rules where the comments do matter. 

---

_@AlexWaygood reviewed on 2024-11-11 12:11_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_literal_member.rs`:106 on 2024-11-11 12:11_

Makes sense.

---

_@AlexWaygood reviewed on 2024-11-11 12:47_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_literal_member.rs`:80 on 2024-11-11 12:47_

Oh, no, this doesn't work, does it. My bad!

---

_@AlexWaygood approved on 2024-11-11 12:47_

---

_Label `fixes` added by @AlexWaygood on 2024-11-11 12:48_

---

_Label `rule` added by @AlexWaygood on 2024-11-11 12:48_

---

_Merged by @AlexWaygood on 2024-11-11 12:48_

---

_Closed by @AlexWaygood on 2024-11-11 12:48_

---

_Branch deleted on 2024-11-11 12:51_

---
