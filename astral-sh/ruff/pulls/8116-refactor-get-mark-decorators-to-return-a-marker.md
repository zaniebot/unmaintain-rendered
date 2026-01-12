```yaml
number: 8116
title: "Refactor `get_mark_decorators` to return a marker name"
type: pull_request
state: merged
author: harupy
labels:
  - internal
assignees: []
merged: true
base: main
head: simplify-get_mark_decorators
created_at: 2023-10-22T04:35:43Z
updated_at: 2023-10-22T14:46:48Z
url: https://github.com/astral-sh/ruff/pull/8116
synced_at: 2026-01-12T15:55:25Z
```

# Refactor `get_mark_decorators` to return a marker name

---

_@harupy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Refactor `get_mark_decorators` to return a marker name to remove `.call_path.last().{unpack|expect}`.

## Test Plan

<!-- How was it tested? -->

Existing tests.

---

_@harupy reviewed on 2023-10-22 04:37_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/helpers.rs`:17 on 2023-10-22 04:37_

The main change.

---

_Comment by @github-actions[bot] on 2023-10-22 04:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@charliermarsh approved on 2023-10-22 14:03_

---

_Label `internal` added by @charliermarsh on 2023-10-22 14:03_

---

_Merged by @charliermarsh on 2023-10-22 14:03_

---

_Closed by @charliermarsh on 2023-10-22 14:03_

---

_Branch deleted on 2023-10-22 14:46_

---
