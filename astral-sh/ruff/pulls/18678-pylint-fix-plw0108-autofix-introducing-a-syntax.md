```yaml
number: 18678
title: "[`pylint`] Fix `PLW0108` autofix introducing a syntax error when the lambda's body contains an assignment expression"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-PLW0108
created_at: 2025-06-14T21:09:12Z
updated_at: 2025-06-26T20:56:55Z
url: https://github.com/astral-sh/ruff/pull/18678
synced_at: 2026-01-12T15:56:23Z
```

# [`pylint`] Fix `PLW0108` autofix introducing a syntax error when the lambda's body contains an assignment expression

---

_@LaBatata101_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR also supresses the fix if the assignment expression target shadows one of the lambda's parameters.

Fixes #18675

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Add regression tests.
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-14 21:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @LaBatata101 on 2025-06-14 22:18_

---

_Review requested from @ntBre by @ntBre on 2025-06-15 17:13_

---

_Label `bug` added by @ntBre on 2025-06-15 17:13_

---

_Label `fixes` added by @ntBre on 2025-06-15 17:13_

---

_Renamed from "[`pylint`] Fix `PLW0108` autofix introducing a syntax error when the lambda's body contained an assignment expression" to "[`pylint`] Fix `PLW0108` autofix introducing a syntax error when the lambda's body contains an assignment expression" by @LaBatata101 on 2025-06-15 21:02_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_lambda.rs`:235 on 2025-06-16 20:56_

Could we use `func.is_named_expr()` here?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_lambda.rs`:242 on 2025-06-16 20:57_

This isn't actually related to your PR, but we could just use `Fix::unsafe_edit` while we're here and not need `Applicability` at all.

---

_@ntBre approved on 2025-06-16 21:14_

Thanks! I just had one nit and an unrelated refactor we could do while we're here, but this looks good to me.

---

_Review requested from @ntBre by @LaBatata101 on 2025-06-26 19:32_

---

_@ntBre approved on 2025-06-26 20:24_

Thanks! I'll merge after the patch release is finished.

---

_Merged by @ntBre on 2025-06-26 20:56_

---

_Closed by @ntBre on 2025-06-26 20:56_

---

_Branch deleted on 2025-06-26 20:56_

---
