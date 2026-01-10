```yaml
number: 14964
title: "[ruff_python_ast] Add name and default functions to TypeParam."
type: pull_request
state: merged
author: rchen152
labels:
  - internal
assignees: []
merged: true
base: main
head: typeparam
created_at: 2024-12-13T23:54:04Z
updated_at: 2024-12-15T22:27:15Z
url: https://github.com/astral-sh/ruff/pull/14964
synced_at: 2026-01-10T20:42:27Z
```

# [ruff_python_ast] Add name and default functions to TypeParam.

---

_Pull request opened by @rchen152 on 2024-12-13 23:54_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This change adds `name` and `default` functions to `TypeParam` to access the corresponding attributes more conveniently. I currently have these as helper functions in code built on top of ruff_python_ast, and they seemed like they might be generally useful.

## Test Plan

Ran the checks listed in CONTRIBUTING.md#development.

---

_Comment by @github-actions[bot] on 2024-12-14 00:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `internal` added by @dylwil3 on 2024-12-14 00:11_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:3390 on 2024-12-14 15:12_

nit: this can be `const`

```suggestion
    pub const fn name(&self) -> &Identifier {
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:3404 on 2024-12-14 15:13_

hmm... I see that this could be useful, but the name could be confusing, since most `default()` methods come from the [`Default` trait](https://doc.rust-lang.org/std/default/trait.Default.html). I don't necessarily have a better suggestion, though...!

---

_@AlexWaygood reviewed on 2024-12-14 15:13_

Thank you!

---

_@MichaReiser reviewed on 2024-12-15 11:59_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:3404 on 2024-12-15 11:59_

I think this is fine, considering that it is aligned with the field name. `Default::default` is also a static method whereas `TypeParam.default` is an instance method

---

_Merged by @MichaReiser on 2024-12-15 12:04_

---

_Closed by @MichaReiser on 2024-12-15 12:04_

---

_Branch deleted on 2024-12-15 22:25_

---

_Comment by @rchen152 on 2024-12-15 22:27_

Thank you both for the speedy reviews!

---
