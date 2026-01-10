```yaml
number: 18472
title: "[`pep8-naming`] Suppress fix for `N804` and `N805` if the recommend name is already used"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-N805
created_at: 2025-06-04T20:40:35Z
updated_at: 2025-06-11T05:58:55Z
url: https://github.com/astral-sh/ruff/pull/18472
synced_at: 2026-01-10T18:45:04Z
```

# [`pep8-naming`] Suppress fix for `N804` and `N805` if the recommend name is already used

---

_Pull request opened by @LaBatata101 on 2025-06-04 20:40_



<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #18459.

Wasn't mentioned in the issue but the `N804` rule is also affected.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Add regression tests.
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-04 20:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-06-04 20:53_

---

_Label `fixes` added by @ntBre on 2025-06-04 20:53_

---

_Review requested from @ntBre by @ntBre on 2025-06-04 20:53_

---

_@MichaReiser reviewed on 2025-06-05 07:20_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_first_argument_name.rs`:304 on 2025-06-05 07:20_

We may want to use https://github.com/astral-sh/ruff/blob/ff6f0b6ab8fef4df5a14651cf9ef86f016ec0367/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs#L137-L139 to also avoid shadowing other variables

---

_Comment by @MichaReiser on 2025-06-11 05:58_

Thank you

---

_Merged by @MichaReiser on 2025-06-11 05:58_

---

_Closed by @MichaReiser on 2025-06-11 05:58_

---
