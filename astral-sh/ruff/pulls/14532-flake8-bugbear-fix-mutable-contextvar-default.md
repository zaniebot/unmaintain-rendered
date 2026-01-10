```yaml
number: 14532
title: "[`flake8-bugbear`] Fix `mutable-contextvar-default (B039)` to resolve annotated function calls properly"
type: pull_request
state: merged
author: harupy
labels:
  - bug
assignees: []
merged: true
base: main
head: issue-14525
created_at: 2024-11-22T14:16:43Z
updated_at: 2024-11-24T02:50:39Z
url: https://github.com/astral-sh/ruff/pull/14532
synced_at: 2026-01-10T20:50:57Z
```

# [`flake8-bugbear`] Fix `mutable-contextvar-default (B039)` to resolve annotated function calls properly

---

_Pull request opened by @harupy on 2024-11-22 14:16_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fix #14525

## Test Plan

<!-- How was it tested? -->

New test cases




---

_Renamed from "[`flake8-bugbear`] Fix `mutable-contextvar-default (B039)` to handle annotated calls" to "[`flake8-bugbear`] Fix `mutable-contextvar-default (B039)` to handle annotated calls properly" by @harupy on 2024-11-22 14:16_

---

_Renamed from "[`flake8-bugbear`] Fix `mutable-contextvar-default (B039)` to handle annotated calls properly" to "[`flake8-bugbear`] Fix `mutable-contextvar-default (B039)` to flag annotated function calls" by @harupy on 2024-11-22 14:22_

---

_Comment by @github-actions[bot] on 2024-11-22 14:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[`flake8-bugbear`] Fix `mutable-contextvar-default (B039)` to flag annotated function calls" to "[`flake8-bugbear`] Fix `mutable-contextvar-default (B039)` to resolve annotated function calls properly" by @harupy on 2024-11-22 14:24_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-22 16:00_

---

_Label `bug` added by @MichaReiser on 2024-11-22 16:00_

---

_@harupy reviewed on 2024-11-23 10:13_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_contextvar_default.rs`:100 on 2024-11-23 10:13_

Unrelated to this PR, but shoud we check if `call.func` is `contextvars.Contextvar` first (and exit if not) before checking if `arguments` has a keyword `default` to avoid unnecessary work (immutability check and extending the immutable expressions)?

---

_@charliermarsh approved on 2024-11-24 02:28_

---

_Merged by @charliermarsh on 2024-11-24 02:29_

---

_Closed by @charliermarsh on 2024-11-24 02:29_

---

_Comment by @charliermarsh on 2024-11-24 02:29_

Thanks!

---

_Branch deleted on 2024-11-24 02:50_

---
