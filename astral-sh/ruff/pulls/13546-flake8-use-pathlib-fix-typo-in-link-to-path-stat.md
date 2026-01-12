```yaml
number: 13546
title: "[`flake8-use-pathlib`] Fix typo in link to Path.stat (PTH116)"
type: pull_request
state: merged
author: echoix
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-09-28T15:00:16Z
updated_at: 2024-09-28T16:02:24Z
url: https://github.com/astral-sh/ruff/pull/13546
synced_at: 2026-01-12T15:55:44Z
```

# [`flake8-use-pathlib`] Fix typo in link to Path.stat (PTH116)

---

_@echoix_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

There was a typo in the links of the docs of PTH116, where Path.stat used to link to Path.group.
Another rule, PTH202, does it correctly: 
https://github.com/astral-sh/ruff/blob/ec72e675d9325d404864c0848e969f59a89090a7/crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_getsize.rs#L33

This PR only fixes a one word typo.

## Test Plan

<!-- How was it tested? -->
I did not test that the doc generation framework picked up these changes, I assume it will do it successfully.

---

_Comment by @github-actions[bot] on 2024-09-28 15:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-09-28 16:01_

Thank you

---

_Label `documentation` added by @charliermarsh on 2024-09-28 16:01_

---

_Merged by @charliermarsh on 2024-09-28 16:01_

---

_Closed by @charliermarsh on 2024-09-28 16:01_

---

_Branch deleted on 2024-09-28 16:02_

---
