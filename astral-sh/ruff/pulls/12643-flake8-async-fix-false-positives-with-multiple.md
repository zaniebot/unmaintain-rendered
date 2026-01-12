```yaml
number: 12643
title: "[flake8-async] Fix false positives with multiple `async with` items (`ASYNC100`)"
type: pull_request
state: merged
author: bluetech
labels: []
assignees: []
merged: true
base: main
head: async100-multi-item
created_at: 2024-08-02T19:43:48Z
updated_at: 2024-08-04T13:20:22Z
url: https://github.com/astral-sh/ruff/pull/12643
synced_at: 2026-01-12T15:55:41Z
```

# [flake8-async] Fix false positives with multiple `async with` items (`ASYNC100`)

---

_@bluetech_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Please see https://github.com/astral-sh/ruff/pull/12605#discussion_r1699957443 for a description of the issue.

They way I fixed it is to get the *last* timeout item in the `with`, and if it's an `async with` and there are items after it, then don't trigger the lint.

## Test Plan

Updated the fixture with some more cases.

---

_@bluetech reviewed on 2024-08-02 19:45_

---

_Review comment by @bluetech on `crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC100.py`:7 on 2024-08-02 19:45_

The `trio` and `anyio` context managers are not `async`. The difference is relevant for this change, so I changed them back to `with`, this way we test both `with` and `async with` (asyncio).

---

_Comment by @github-actions[bot] on 2024-08-02 19:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-08-02 21:19_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC100.py`:7 on 2024-08-02 21:19_

Thanks, that's my bad!

---

_@charliermarsh approved on 2024-08-02 21:21_

Clever, thanks!

---

_Merged by @charliermarsh on 2024-08-02 21:25_

---

_Closed by @charliermarsh on 2024-08-02 21:25_

---

_Branch deleted on 2024-08-04 13:20_

---
