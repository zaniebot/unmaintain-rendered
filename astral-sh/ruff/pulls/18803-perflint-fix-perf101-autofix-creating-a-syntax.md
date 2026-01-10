```yaml
number: 18803
title: "[`perflint`] Fix `PERF101` autofix creating a syntax error and mark autofix as unsafe if there are comments in the `list` call expr"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-PERF101
created_at: 2025-06-19T20:00:31Z
updated_at: 2025-06-23T13:18:25Z
url: https://github.com/astral-sh/ruff/pull/18803
synced_at: 2026-01-10T18:39:09Z
```

# [`perflint`] Fix `PERF101` autofix creating a syntax error and mark autofix as unsafe if there are comments in the `list` call expr

---

_Pull request opened by @LaBatata101 on 2025-06-19 20:00_



<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #18783 and #18784
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Add  regression tests
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-19 20:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-06-23 11:48_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/unnecessary_list_cast.rs`:43 on 2025-06-23 11:48_

```suggestion
/// `list()` call, as comments may be removed.
```

---

_@MichaReiser approved on 2025-06-23 11:49_

Thank you

---

_Label `bug` added by @MichaReiser on 2025-06-23 11:49_

---

_Label `fixes` added by @MichaReiser on 2025-06-23 11:49_

---

_Merged by @MichaReiser on 2025-06-23 11:51_

---

_Closed by @MichaReiser on 2025-06-23 11:51_

---

_Branch deleted on 2025-06-23 13:18_

---
