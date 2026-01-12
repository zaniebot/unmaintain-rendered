```yaml
number: 8329
title: Respect --extend-per-file-ignores on the CLI
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: charlie/extends
created_at: 2023-10-29T22:48:30Z
updated_at: 2023-10-30T01:14:10Z
url: https://github.com/astral-sh/ruff/pull/8329
synced_at: 2026-01-10T23:40:55Z
```

# Respect --extend-per-file-ignores on the CLI

---

_Pull request opened by @charliermarsh on 2023-10-29 22:48_

## Summary

This field was being dropped from the CLI.

Closes https://github.com/astral-sh/ruff/issues/8328.

---

_Label `bug` added by @charliermarsh on 2023-10-29 22:48_

---

_Label `cli` added by @charliermarsh on 2023-10-29 22:48_

---

_Review requested from @zanieb by @charliermarsh on 2023-10-29 22:58_

---

_Comment by @github-actions[bot] on 2023-10-29 23:17_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2023-10-30 00:11_

We could consider removing the CLI option instead. It doesn't seem to have been used widely ;)

---

_Merged by @charliermarsh on 2023-10-30 00:44_

---

_Closed by @charliermarsh on 2023-10-30 00:44_

---

_Branch deleted on 2023-10-30 00:44_

---

_Comment by @ofek on 2023-10-30 01:14_

Thank you for the fix, I'm about to use this in Hatch!

---
