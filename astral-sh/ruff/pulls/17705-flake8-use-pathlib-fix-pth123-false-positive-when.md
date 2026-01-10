```yaml
number: 17705
title: "[`flake8-use-pathlib`] Fix `PTH123` false positive when `open` is passed a file descriptor from a function call"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-PTH123
created_at: 2025-04-29T13:54:07Z
updated_at: 2025-04-30T20:55:03Z
url: https://github.com/astral-sh/ruff/pull/17705
synced_at: 2026-01-10T19:03:00Z
```

# [`flake8-use-pathlib`] Fix `PTH123` false positive when `open` is passed a file descriptor from a function call

---

_Pull request opened by @LaBatata101 on 2025-04-29 13:54_



<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Includes minor changes to the semantic type inference to help detect the return type of function call.

Fixes #17691

## Test Plan

Snapshot tests


---

_Comment by @github-actions[bot] on 2025-04-29 14:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @LaBatata101 on 2025-04-29 14:50_

Why the CI failed here?

---

_Comment by @AlexWaygood on 2025-04-29 15:07_

> Why the CI failed here?

I re-ran it and it passed. I assume it's just a fluke :-)

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/replaceable_by_pathlib.rs`:212 on 2025-04-29 20:14_

Can we add a few test cases with byte strings? This looks right to me, but I don't think we have any tests exercising it.

---

_@ntBre approved on 2025-04-29 20:15_

Thanks, this looks great to me! Let's just add a few test cases for bytestrings, but otherwise this is good to go.

---

_Label `bug` added by @ntBre on 2025-04-29 20:17_

---

_Label `rule` added by @ntBre on 2025-04-29 20:17_

---

_@LaBatata101 reviewed on 2025-04-29 20:43_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/replaceable_by_pathlib.rs`:212 on 2025-04-29 20:43_

I've added the test cases

---

_@ntBre reviewed on 2025-04-29 20:49_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/replaceable_by_pathlib.rs`:212 on 2025-04-29 20:49_

Looks great, thanks!

---

_Merged by @ntBre on 2025-04-29 20:51_

---

_Closed by @ntBre on 2025-04-29 20:51_

---

_Branch deleted on 2025-04-30 20:55_

---
