```yaml
number: 18862
title: "[`ruff`] Trigger `RUF037` for empty string and byte strings"
type: pull_request
state: merged
author: LaBatata101
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-RUF037
created_at: 2025-06-22T13:54:58Z
updated_at: 2025-06-24T13:01:09Z
url: https://github.com/astral-sh/ruff/pull/18862
synced_at: 2026-01-10T18:39:09Z
```

# [`ruff`] Trigger `RUF037` for empty string and byte strings

---

_Pull request opened by @LaBatata101 on 2025-06-22 13:54_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
Fixes https://github.com/astral-sh/ruff/issues/18854
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Add regression test
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-22 14:06_

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

_@MichaReiser reviewed on 2025-06-23 07:43_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_literal_within_deque_call.rs`:103 on 2025-06-23 07:43_

We should probably also handle f-strings if we handle byte strings

---

_Review requested from @MichaReiser by @LaBatata101 on 2025-06-23 20:19_

---

_@MichaReiser approved on 2025-06-24 06:26_

Thank you

---

_Label `rule` added by @MichaReiser on 2025-06-24 06:26_

---

_Label `preview` added by @MichaReiser on 2025-06-24 06:26_

---

_Merged by @MichaReiser on 2025-06-24 06:26_

---

_Closed by @MichaReiser on 2025-06-24 06:26_

---

_Branch deleted on 2025-06-24 13:01_

---
