```yaml
number: 18390
title: "[`pyupgrade`] Make fix unsafe if it deletes comments (`UP050`)"
type: pull_request
state: merged
author: chirizxc
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix/useless-class-metaclass-type
created_at: 2025-05-30T18:59:06Z
updated_at: 2025-06-04T14:53:39Z
url: https://github.com/astral-sh/ruff/pull/18390
synced_at: 2026-01-10T18:45:04Z
```

# [`pyupgrade`] Make fix unsafe if it deletes comments (`UP050`)

---

_Pull request opened by @chirizxc on 2025-05-30 18:59_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
/closes #18387
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
update snapshots
<!-- How was it tested? -->


---

_Renamed from "[`pyupgrade`] make fix unsafe if it deletes comments (UP050, useless-…" to "[`pyupgrade`] make fix unsafe if it deletes comments (UP050, useless-class-metaclass-type)" by @chirizxc on 2025-05-30 18:59_

---

_Label `bug` added by @ntBre on 2025-05-30 19:06_

---

_Label `fixes` added by @ntBre on 2025-05-30 19:06_

---

_Review requested from @ntBre by @ntBre on 2025-05-30 19:06_

---

_Comment by @github-actions[bot] on 2025-05-30 19:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/useless_class_metaclass_type.rs`:82 on 2025-06-02 21:12_

Do we need this `isolate` call?

---

_@ntBre reviewed on 2025-06-02 21:13_

Thanks, this one looks good too! Just the same question about the edit isolation as in https://github.com/astral-sh/ruff/pull/18393.

---

_@ntBre approved on 2025-06-03 13:09_

---

_Renamed from "[`pyupgrade`] make fix unsafe if it deletes comments (UP050, useless-class-metaclass-type)" to "[`pyupgrade`] Make fix unsafe if it deletes comments (`UP050`)" by @ntBre on 2025-06-03 13:10_

---

_Merged by @ntBre on 2025-06-03 13:10_

---

_Closed by @ntBre on 2025-06-03 13:10_

---

_Branch deleted on 2025-06-04 14:53_

---
