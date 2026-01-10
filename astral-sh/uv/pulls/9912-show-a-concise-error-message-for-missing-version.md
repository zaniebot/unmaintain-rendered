```yaml
number: 9912
title: Show a concise error message for missing version field
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/version
created_at: 2024-12-15T14:17:06Z
updated_at: 2024-12-15T15:27:44Z
url: https://github.com/astral-sh/uv/pull/9912
synced_at: 2026-01-10T12:00:01Z
```

# Show a concise error message for missing version field

---

_Pull request opened by @charliermarsh on 2024-12-15 14:17_

## Summary

This now looks like:

```
error: Failed to parse: `pyproject.toml`
  Caused by: TOML parse error at line 1, column 1
  |
1 | [project]
  | ^^^^^^^^^
`pyproject.toml` is using the `[project]` table, but the required `project.version` field is neither set nor present in the `project.dynamic` list
```

Closes https://github.com/astral-sh/uv/issues/9910.


---

_Label `error messages` added by @charliermarsh on 2024-12-15 14:35_

---

_Merged by @charliermarsh on 2024-12-15 15:27_

---

_Closed by @charliermarsh on 2024-12-15 15:27_

---

_Branch deleted on 2024-12-15 15:27_

---
