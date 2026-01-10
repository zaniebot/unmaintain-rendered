```yaml
number: 14318
title: Remove unused flags and functions from the semantic model
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - linter
assignees: []
merged: true
base: main
head: alex/unused-functions
created_at: 2024-11-13T12:55:27Z
updated_at: 2024-11-13T17:45:08Z
url: https://github.com/astral-sh/ruff/pull/14318
synced_at: 2026-01-10T20:50:57Z
```

# Remove unused flags and functions from the semantic model

---

_Pull request opened by @AlexWaygood on 2024-11-13 12:55_

## Summary

I noticed while reviewing https://github.com/astral-sh/ruff/pull/14311 that some of the functions publicly exposed by the semantic model are never used anywhere, and some of the semantic-model flags set by the AST visitor but never read.

I'm not sure if this PR is really a good idea or not... some of these flags and functions are not used right now, but there's a chance we might use them in the future? Not sure.

I feel most confident about the changes in `crates/ruff_python_semantic/src/reference.rs`, `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs` and `crates/ruff_linter/src/rules/pylint/rules/import_private_name.rs`, since the `ResolvedReference` struct already only exposes a subset of the interface exposed by the `SemanticModel`. It doesn't seem like we're trying with that struct to publicly expose information regarding all flags the `SemanticModel` stores internally.

## Test Plan

- `cargo test -p ruff_linter`
- `cargo test -p ruff_python_semantic`

---

_Label `internal` added by @AlexWaygood on 2024-11-13 12:55_

---

_Label `linter` added by @AlexWaygood on 2024-11-13 12:59_

---

_Comment by @github-actions[bot] on 2024-11-13 13:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:27 on 2024-11-13 16:42_

What's the reason for removing `in_complex_string_type_definition`. Is it because the flag was never set? Or is it because you merged the simple and complex flags?

---

_@MichaReiser reviewed on 2024-11-13 16:42_

---

_@MichaReiser reviewed on 2024-11-13 16:51_

I find this PR somewhat difficult to review because it scrambles together multiple changes that have very little in common, other than that they unused. 

I don't mind approving because I'm in favor of removing unused methods together with unnecessarily tracked data (but we shouldn't remove the methods but still keep tracking the data). However, I don't feel like I can give you a proper review without some added explanation or splitting the change into a smaller PR.

---

_@charliermarsh approved on 2024-11-13 17:15_

This is good IMO.

---

_@charliermarsh reviewed on 2024-11-13 17:17_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:1756 on 2024-11-13 17:17_

I think we should leave this as-is for any flags that still exist. It's nice to have a consistent interface even if unused. (But of course we can remove for any flags that we've removed.)

---

_@charliermarsh reviewed on 2024-11-13 17:20_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:2231 on 2024-11-13 17:20_

I think we should leave this, actually. It was added in https://github.com/astral-sh/ruff/pull/11315 though I don't think it was "finished".

---

_@AlexWaygood reviewed on 2024-11-13 17:20_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:27 on 2024-11-13 17:20_

I removed the `ResolvedReference:in_simple_string_type_definition()` and `ResolvedReference::in_complex_string_type_definition()` methods in favour of a single `ResolvedReference::in_string_type_definition()` method. None of our rules need to distinguish (at least not currently) between resolved read references that occur within string type definitions that use concatenated strings and resolved read references that occur within string type definitions that do not use concatenated strings. 

---

_@charliermarsh reviewed on 2024-11-13 17:21_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/mod.rs`:1797 on 2024-11-13 17:21_

Good to remove, looks like we got rid of this `BindingKind` at some point: https://github.com/astral-sh/ruff/pull/9967

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:2231 on 2024-11-13 17:22_

Oh, good point -- https://github.com/astral-sh/ruff/issues/10347 is still open

---

_@AlexWaygood reviewed on 2024-11-13 17:22_

---

_@charliermarsh reviewed on 2024-11-13 17:23_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:27 on 2024-11-13 17:23_

Yeah this seems ok. It looks like `in_complex_string_type_definition` on `SemanticModel` remains and _is_ necessary though, is that right?

---

_@AlexWaygood reviewed on 2024-11-13 17:23_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:27 on 2024-11-13 17:23_

> It looks like `in_complex_string_type_definition` on `SemanticModel` remains and _is_ necessary though, is that right?

correct

---

_@AlexWaygood reviewed on 2024-11-13 17:31_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:2231 on 2024-11-13 17:31_

added it back

---

_Comment by @AlexWaygood on 2024-11-13 17:32_

thanks @charliermarsh!

---

_Merged by @AlexWaygood on 2024-11-13 17:35_

---

_Closed by @AlexWaygood on 2024-11-13 17:35_

---

_Branch deleted on 2024-11-13 17:35_

---
