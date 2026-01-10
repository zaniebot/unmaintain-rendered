```yaml
number: 13517
title: "Disable the `typeset` plugin"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/typeset
created_at: 2024-09-25T23:54:05Z
updated_at: 2024-09-26T10:27:55Z
url: https://github.com/astral-sh/ruff/pull/13517
synced_at: 2026-01-10T20:59:36Z
```

# Disable the `typeset` plugin

---

_Pull request opened by @charliermarsh on 2024-09-25 23:54_

## Summary

There seems to be a bad interaction between enabling anchorlinks and the `typeset` plugin. I think the former is more important than the latter... so disabling the latter for now.

## Test Plan

Before:

![Screenshot 2024-09-25 at 7 53 21 PM](https://github.com/user-attachments/assets/bf7c70bb-19ab-4ece-9709-4c297f8ba67b)

After:

![Screenshot 2024-09-25 at 7 53 12 PM](https://github.com/user-attachments/assets/e767a575-1664-4288-aecb-82e8b1b1a7bd)




---

_Label `bug` added by @charliermarsh on 2024-09-25 23:54_

---

_Label `documentation` added by @charliermarsh on 2024-09-25 23:54_

---

_Merged by @charliermarsh on 2024-09-25 23:58_

---

_Closed by @charliermarsh on 2024-09-25 23:58_

---

_Branch deleted on 2024-09-25 23:58_

---

_Label `bug` removed by @MichaReiser on 2024-09-26 10:27_

---
