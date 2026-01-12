```yaml
number: 14272
title: "[`flake8-pyi`] Improve autofix for nested and mixed type unions `unnecessary-type-union` (`PYI055`)"
type: pull_request
state: merged
author: sbrugman
labels:
  - rule
  - fixes
assignees: []
merged: true
base: main
head: pyi055-improve-autofix
created_at: 2024-11-11T12:24:28Z
updated_at: 2024-11-12T20:41:32Z
url: https://github.com/astral-sh/ruff/pull/14272
synced_at: 2026-01-12T15:55:47Z
```

# [`flake8-pyi`] Improve autofix for nested and mixed type unions `unnecessary-type-union` (`PYI055`)

---

_@sbrugman_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR improves the fix for `PYI055` to be able to handle nested and mixed type unions.

It also marks the fix as unsafe when comments are present. 
 
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-11-11 12:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @AlexWaygood on 2024-11-11 13:57_

---

_Label `fixes` added by @AlexWaygood on 2024-11-11 13:57_

---

_@sbrugman reviewed on 2024-11-11 14:15_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_type_union.rs`:197 on 2024-11-11 14:15_

Draft because this `return` statement is bugging me.

---

_Marked ready for review by @sbrugman on 2024-11-11 17:03_

---

_Review requested from @AlexWaygood by @sbrugman on 2024-11-11 17:03_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_type_union.rs`:112 on 2024-11-12 10:04_

What's the reason what we need to clone the `type_expr` if we only take the range.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_type_union.rs`:207 on 2024-11-12 10:19_

Hmm, I looked through all the `intersects` usages and I only found one exception where we set `Applicability::Unsafe` if the node contains comments. All other usages don't offer a fix instead. I think we should do the same here.

Could you take a look if we should use `has_comments` instead?



---

_@MichaReiser requested changes on 2024-11-12 10:20_

Nice.

The same as for other fixes. I think we should not offer the fix if the range contains comments instead of marking the fix as unsafe.

---

_@MichaReiser approved on 2024-11-12 10:20_

---

_@sbrugman reviewed on 2024-11-12 11:59_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_type_union.rs`:207 on 2024-11-12 11:59_

`has_comments` is just wrapping `intersects` that includes leading and trailing content.

For type annotations I cannot come up with a case where the fix is changing the leading/trailing content:

```diff
   a: ( # comment 1
-    type[int] | type[float] ) = 3
+    type[int | float] ) =  3
```

I'm not absolutely sure though.

---

_@sbrugman reviewed on 2024-11-12 13:04_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_type_union.rs`:112 on 2024-11-12 13:04_

It's not required, nice catch

---

_Merged by @charliermarsh on 2024-11-12 20:33_

---

_Closed by @charliermarsh on 2024-11-12 20:33_

---

_Branch deleted on 2024-11-12 20:41_

---
