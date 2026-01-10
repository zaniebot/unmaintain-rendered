```yaml
number: 12261
title: "[`flake8-async`] Update `ASYNC110` to match upstream"
type: pull_request
state: merged
author: augustelalande
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: async110
created_at: 2024-07-09T22:29:58Z
updated_at: 2024-07-10T02:24:54Z
url: https://github.com/astral-sh/ruff/pull/12261
synced_at: 2026-01-10T21:47:02Z
```

# [`flake8-async`] Update `ASYNC110` to match upstream

---

_Pull request opened by @augustelalande on 2024-07-09 22:29_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Update the name of `ASYNC110` to match [upstream](https://flake8-async.readthedocs.io/en/latest/rules.html).

Also update the functionality to match upstream by adding support for `asyncio` and `anyio` (gated behind preview).

Part of https://github.com/astral-sh/ruff/issues/12039.

## Test Plan

Added tests for `asyncio` and `anyio`


---

_@charliermarsh approved on 2024-07-10 00:17_

Awesome, thank you.

---

_Label `rule` added by @charliermarsh on 2024-07-10 00:17_

---

_Label `preview` added by @charliermarsh on 2024-07-10 00:17_

---

_Merged by @charliermarsh on 2024-07-10 00:17_

---

_Closed by @charliermarsh on 2024-07-10 00:17_

---

_Branch deleted on 2024-07-10 01:50_

---
