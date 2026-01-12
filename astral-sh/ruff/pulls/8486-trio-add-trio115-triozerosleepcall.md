```yaml
number: 8486
title: "[`TRIO`] Add `TRIO115`: TrioZeroSleepCall"
type: pull_request
state: merged
author: qdegraaf
labels:
  - rule
assignees: []
merged: true
base: main
head: feat/TRIO115
created_at: 2023-11-04T12:32:20Z
updated_at: 2023-11-06T15:53:35Z
url: https://github.com/astral-sh/ruff/pull/8486
synced_at: 2026-01-10T23:40:55Z
```

# [`TRIO`] Add `TRIO115`: TrioZeroSleepCall

---

_Pull request opened by @qdegraaf on 2023-11-04 12:32_

## Summary

Adds `TRIO115` from the [flake8-trio plugin](https://github.com/Zac-HD/flake8-trio). 

## Test Plan

Added a new fixture, based on [the one from upstream plugin](https://github.com/Zac-HD/flake8-trio/blob/main/tests/eval_files/trio115.py)

## Issue link

Relates to: https://github.com/astral-sh/ruff/issues/8451


---

_@qdegraaf reviewed on 2023-11-04 12:33_

---

_Review comment by @qdegraaf on `crates/ruff_linter/src/rules/flake8_trio/rules/zero_sleep_call.rs`:68 on 2023-11-04 12:33_

I couldn't find a more compact way to do this yet. In case there is one somewhere in the repo already, happy to hear about it and refactor this.

---

_@qdegraaf reviewed on 2023-11-04 12:35_

---

_Review comment by @qdegraaf on `crates/ruff_linter/src/rules/flake8_trio/rules/zero_sleep_call.rs`:14 on 2023-11-04 12:35_

Interpreted from upstream plugin, couldn't find additional info in docs or PRs on the reason. All input is welcome

---

_Comment by @github-actions[bot] on 2023-11-04 12:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @charliermarsh by @charliermarsh on 2023-11-06 01:09_

---

_Label `rule` added by @charliermarsh on 2023-11-06 01:09_

---

_@charliermarsh approved on 2023-11-06 01:12_

---

_Merged by @charliermarsh on 2023-11-06 01:19_

---

_Closed by @charliermarsh on 2023-11-06 01:19_

---

_Branch deleted on 2023-11-06 15:53_

---
