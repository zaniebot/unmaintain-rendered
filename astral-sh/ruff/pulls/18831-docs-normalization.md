```yaml
number: 18831
title: Docs normalization
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs-normalization
created_at: 2025-06-20T18:56:41Z
updated_at: 2025-06-20T20:56:43Z
url: https://github.com/astral-sh/ruff/pull/18831
synced_at: 2026-01-10T18:39:09Z
```

# Docs normalization

---

_Pull request opened by @MeGaGiGaGon on 2025-06-20 18:56_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

While working on a detector for #15584, I noticed several inconsistencies in the docs, so this PR fixes them.
- Blank line between doc comment and `#[derive(ViolationMetadata)]`
- Empty doc comment before `#[derive(ViolationMetadata)]`
- `## Fix Safety` instead of `## Fix safety`

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Review requested from @AlexWaygood by @MeGaGiGaGon on 2025-06-20 18:56_

---

_Label `documentation` added by @ntBre on 2025-06-20 19:05_

---

_Comment by @github-actions[bot] on 2025-06-20 19:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_for_self.rs`:1 on 2025-06-20 20:41_

I'm not sure there's a need to change this file -- we just end up with a very small `Fix safety` section, and I think the content in that paragraph naturally fits with the content of the preceding two paragraphs. I don't think it's essential that every rule is totally consistent in the docs sections it has.

---

_@AlexWaygood reviewed on 2025-06-20 20:42_

Thanks! One of these changes feels unnecessary to me, but the rest LGTM :-)

---

_@MeGaGiGaGon reviewed on 2025-06-20 20:52_

---

_Review comment by @MeGaGiGaGon on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_for_self.rs`:1 on 2025-06-20 20:52_

The main reason I did is for the consistency - like what I've been doing, if you are writing a tool to try and detect all rules missing the section, it's a one-off edge case, since in the searching I did I could not find another rule that had this format.

---

_@AlexWaygood reviewed on 2025-06-20 20:55_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_for_self.rs`:1 on 2025-06-20 20:55_

Eh, fair enough. It would certainly be nice to enforce (one day...) in CI that all rules which are sometimes unsafe have a `Fix safety` section in their docs explaining why.

---

_@AlexWaygood approved on 2025-06-20 20:55_

---

_Merged by @AlexWaygood on 2025-06-20 20:56_

---

_Closed by @AlexWaygood on 2025-06-20 20:56_

---

_Branch deleted on 2025-06-20 20:56_

---
