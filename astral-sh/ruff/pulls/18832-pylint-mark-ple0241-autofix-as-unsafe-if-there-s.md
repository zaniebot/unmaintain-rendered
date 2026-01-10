```yaml
number: 18832
title: "[`pylint`] Mark `PLE0241` autofix as unsafe if there's comments in the base classes"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-PLE0241
created_at: 2025-06-20T20:08:37Z
updated_at: 2025-06-21T20:15:50Z
url: https://github.com/astral-sh/ruff/pull/18832
synced_at: 2026-01-10T18:39:09Z
```

# [`pylint`] Mark `PLE0241` autofix as unsafe if there's comments in the base classes

---

_Pull request opened by @LaBatata101 on 2025-06-20 20:08_



<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
Fixes https://github.com/astral-sh/ruff/issues/18814
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Add regression test
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-20 20:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @MichaReiser on 2025-06-21 17:11_

---

_Label `fixes` added by @MichaReiser on 2025-06-21 17:11_

---

_@MichaReiser reviewed on 2025-06-21 17:11_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/duplicate_bases.rs`:40 on 2025-06-21 17:11_

```suggestion
/// base classes, as comments may be removed..
```

---

_@MichaReiser reviewed on 2025-06-21 17:12_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/duplicate_bases.rs`:55 on 2025-06-21 17:12_

```suggestion
///
/// ## References
```

---

_@MichaReiser approved on 2025-06-21 17:12_

Thank you

---

_Merged by @MichaReiser on 2025-06-21 17:20_

---

_Closed by @MichaReiser on 2025-06-21 17:20_

---

_Branch deleted on 2025-06-21 20:15_

---
