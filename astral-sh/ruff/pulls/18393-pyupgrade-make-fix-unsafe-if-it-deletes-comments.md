```yaml
number: 18393
title: "[`pyupgrade`] Make fix unsafe if it deletes comments (`UP004`)"
type: pull_request
state: merged
author: chirizxc
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix/useless-object-inheritance
created_at: 2025-05-30T19:31:40Z
updated_at: 2025-06-04T14:53:31Z
url: https://github.com/astral-sh/ruff/pull/18393
synced_at: 2026-01-10T18:45:04Z
```

# [`pyupgrade`] Make fix unsafe if it deletes comments (`UP004`)

---

_Pull request opened by @chirizxc on 2025-05-30 19:31_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
https://github.com/astral-sh/ruff/issues/18387#issuecomment-2923039331
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
update snapshots
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-05-30 19:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @ntBre on 2025-05-30 23:26_

---

_Label `bug` added by @ntBre on 2025-05-30 23:26_

---

_Label `fixes` added by @ntBre on 2025-05-30 23:26_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/useless_object_inheritance.rs`:82 on 2025-06-02 21:11_

Do we need this `isolate` call? It doesn't look like the fix was isolated before.

---

_@ntBre reviewed on 2025-06-02 21:11_

Thanks! Just one question, but this looks good to me otherwise.

---

_@chirizxc reviewed on 2025-06-03 06:59_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/pyupgrade/rules/useless_object_inheritance.rs`:82 on 2025-06-03 06:59_

> Do we need this `isolate` call? It doesn't look like the fix was isolated before.

i don't think so

---

_@ntBre approved on 2025-06-03 13:09_

---

_Renamed from "[`pyupgrade`] make fix unsafe if it deletes comments (UP004, useless-object-inheritance)" to "[`pyupgrade`] Make fix unsafe if it deletes comments (`UP004`)" by @ntBre on 2025-06-03 13:09_

---

_Merged by @ntBre on 2025-06-03 13:09_

---

_Closed by @ntBre on 2025-06-03 13:09_

---

_Branch deleted on 2025-06-04 14:53_

---
