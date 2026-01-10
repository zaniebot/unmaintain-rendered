```yaml
number: 14871
title: "[`pyupgrade`] Remove unreachable code in `UP015` implementation"
type: pull_request
state: merged
author: harupy
labels:
  - internal
assignees: []
merged: true
base: main
head: UP015-unreachable
created_at: 2024-12-09T13:43:53Z
updated_at: 2024-12-09T14:57:45Z
url: https://github.com/astral-sh/ruff/pull/14871
synced_at: 2026-01-10T20:42:27Z
```

# [`pyupgrade`] Remove unreachable code in `UP015` implementation

---

_Pull request opened by @harupy on 2024-12-09 13:43_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

`crates/ruff_linter/src/rules/pyupgrade/rules/redundant_open_modes.rs` has unreachable code. This PR removes it.

## Test Plan

<!-- How was it tested? -->

No behavior change. No test.


---

_@harupy reviewed on 2024-12-09 13:45_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/pyupgrade/rules/redundant_open_modes.rs`:96 on 2024-12-09 13:45_

This block is unreachable. If `call.arguments.find_argument("mode", 1)` is None, `call.arguments.find_keyword("mode")` should also be None.

---

_Comment by @github-actions[bot] on 2024-12-09 13:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@harupy reviewed on 2024-12-09 13:55_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/pyupgrade/rules/redundant_open_modes.rs`:74 on 2024-12-09 13:55_

Should I rewrite this block using `let-else`?

---

_Renamed from "Remove unreachable code in `UP015`" to "[`pyupgrade`] Remove unreachable code in `UP015`" by @harupy on 2024-12-09 14:05_

---

_@harupy reviewed on 2024-12-09 14:26_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/pyupgrade/rules/redundant_open_modes.rs`:96 on 2024-12-09 14:26_

https://github.com/astral-sh/ruff/commit/fd40864924c757ee95d1356c48431358de716d69 introduced `find_argument` in this file but it didn't remove the `None` branch.

---

_@AlexWaygood reviewed on 2024-12-09 14:34_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/redundant_open_modes.rs`:96 on 2024-12-09 14:34_

Great catch!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/redundant_open_modes.rs`:74 on 2024-12-09 14:34_

Yes please, I think that would be cleaner :-)

---

_@AlexWaygood reviewed on 2024-12-09 14:34_

---

_@AlexWaygood approved on 2024-12-09 14:54_

Great, thank you!

---

_Label `internal` added by @AlexWaygood on 2024-12-09 14:54_

---

_Renamed from "[`pyupgrade`] Remove unreachable code in `UP015`" to "[`pyupgrade`] Remove unreachable code in `UP015` implementation" by @AlexWaygood on 2024-12-09 14:54_

---

_Merged by @AlexWaygood on 2024-12-09 14:54_

---

_Closed by @AlexWaygood on 2024-12-09 14:54_

---

_Branch deleted on 2024-12-09 14:56_

---
