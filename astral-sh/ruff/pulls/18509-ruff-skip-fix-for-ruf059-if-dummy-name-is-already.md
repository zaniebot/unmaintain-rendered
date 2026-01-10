```yaml
number: 18509
title: "[`ruff`] skip fix for `RUF059` if dummy name is already bound (unused-unpacked-variable)"
type: pull_request
state: merged
author: chirizxc
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix/unused-unpacked-variable
created_at: 2025-06-06T18:19:13Z
updated_at: 2025-06-11T08:25:00Z
url: https://github.com/astral-sh/ruff/pull/18509
synced_at: 2026-01-10T18:45:04Z
```

# [`ruff`] skip fix for `RUF059` if dummy name is already bound (unused-unpacked-variable)

---

_Pull request opened by @chirizxc on 2025-06-06 18:19_



<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
/closes #18507
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-06 18:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-06-06 18:37_

---

_Label `fixes` added by @ntBre on 2025-06-06 18:37_

---

_@MichaReiser reviewed on 2025-06-10 11:13_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unused_unpacked_variable.rs`:74 on 2025-06-10 11:13_

I think we could use the following here (which is what we use in other places to test shadowing)

https://github.com/astral-sh/ruff/blob/9925910a29643ff1f29eafeadc2b1d26812e4993/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs#L137-L139


---

_Comment by @MichaReiser on 2025-06-11 05:58_

Thank you

---

_Merged by @MichaReiser on 2025-06-11 05:58_

---

_Closed by @MichaReiser on 2025-06-11 05:58_

---

_Branch deleted on 2025-06-11 08:25_

---
