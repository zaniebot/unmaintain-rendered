```yaml
number: 9846
title: "Add ``--preview`` to instruction for running newly added tests"
type: pull_request
state: merged
author: DanielNoord
labels:
  - documentation
assignees: []
merged: true
base: main
head: add-preview
created_at: 2024-02-05T22:12:10Z
updated_at: 2024-02-06T07:02:15Z
url: https://github.com/astral-sh/ruff/pull/9846
synced_at: 2026-01-10T22:57:09Z
```

# Add ``--preview`` to instruction for running newly added tests

---

_Pull request opened by @DanielNoord on 2024-02-05 22:12_



## Summary

This surprised me while working on adding a test. I thought about adding an additional `note`, but how often is this incorrect? In general, people reading the contributing guidelines probably want to enable this flag and those who don't will know enough about the testing setup to have their own commands/aliases.

## Test Plan

Ran CI on local fork and got an all green.


---

_Comment by @charliermarsh on 2024-02-06 00:33_

Thank you!

---

_Merged by @charliermarsh on 2024-02-06 00:33_

---

_Closed by @charliermarsh on 2024-02-06 00:33_

---

_Label `documentation` added by @charliermarsh on 2024-02-06 00:33_

---

_Branch deleted on 2024-02-06 07:02_

---
