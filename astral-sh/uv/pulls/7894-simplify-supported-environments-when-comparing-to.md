```yaml
number: 7894
title: Simplify supported environments when comparing to lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/supported
created_at: 2024-10-03T12:51:32Z
updated_at: 2024-10-03T13:15:10Z
url: https://github.com/astral-sh/uv/pull/7894
synced_at: 2026-01-10T12:53:58Z
```

# Simplify supported environments when comparing to lockfile

---

_Pull request opened by @charliermarsh on 2024-10-03 12:51_

## Summary

If a supported environment includes a Python marker, we don't simplify it out, despite _storing_ the simplified markers. This PR modifies the validation code to compare simplified to simplified markers.

Closes https://github.com/astral-sh/uv/issues/7876.


---

_Label `bug` added by @charliermarsh on 2024-10-03 12:51_

---

_Merged by @charliermarsh on 2024-10-03 13:15_

---

_Closed by @charliermarsh on 2024-10-03 13:15_

---

_Branch deleted on 2024-10-03 13:15_

---
