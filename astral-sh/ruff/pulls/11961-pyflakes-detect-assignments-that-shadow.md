```yaml
number: 11961
title: "[`pyflakes`] Detect assignments that shadow definitions (`F811`)"
type: pull_request
state: merged
author: ukyen8
labels:
  - bug
assignees: []
merged: true
base: main
head: bugfix-f811
created_at: 2024-06-21T09:38:52Z
updated_at: 2024-06-23T17:29:33Z
url: https://github.com/astral-sh/ruff/pull/11961
synced_at: 2026-01-10T21:56:00Z
```

# [`pyflakes`] Detect assignments that shadow definitions (`F811`)

---

_Pull request opened by @ukyen8 on 2024-06-21 09:38_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR updates `F811` rule to include assignment as possible shadowed binding. This will fix issue: #11828 .

## Test Plan

Add a test file, F811_30.py, which includes a redefinition after an assignment and a verified snapshot file.


---

_Marked ready for review by @ukyen8 on 2024-06-21 12:04_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-23 16:27_

---

_Renamed from "fix(F811): assignment can also be shadowed binding" to "[`pyflakes`] Detect assignments that shadow definitions (`F811`)" by @charliermarsh on 2024-06-23 17:13_

---

_Label `bug` added by @charliermarsh on 2024-06-23 17:13_

---

_@charliermarsh approved on 2024-06-23 17:13_

Thanks!

---

_Merged by @charliermarsh on 2024-06-23 17:29_

---

_Closed by @charliermarsh on 2024-06-23 17:29_

---
