```yaml
number: 9215
title: "Only install the specified project with `--frozen --package` in legacy non-`[project]` workspaces"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/leg
created_at: 2024-11-19T03:07:02Z
updated_at: 2024-11-19T03:18:01Z
url: https://github.com/astral-sh/uv/pull/9215
synced_at: 2026-01-12T16:08:42Z
```

# Only install the specified project with `--frozen --package` in legacy non-`[project]` workspaces

---

_@charliermarsh_

## Summary

We missed the case in which the user has a legacy non-`[project]` root -- we were always installing all members.

Closes https://github.com/astral-sh/uv/issues/9214.


---

_Label `bug` added by @charliermarsh on 2024-11-19 03:07_

---

_Marked ready for review by @charliermarsh on 2024-11-19 03:07_

---

_Merged by @charliermarsh on 2024-11-19 03:18_

---

_Closed by @charliermarsh on 2024-11-19 03:18_

---

_Branch deleted on 2024-11-19 03:18_

---
