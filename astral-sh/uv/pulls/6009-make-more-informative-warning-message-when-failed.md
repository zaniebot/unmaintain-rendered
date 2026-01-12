```yaml
number: 6009
title: Make more informative warning message when failed to parse pyproject.toml
type: pull_request
state: merged
author: flyaroundme
labels:
  - error messages
assignees: []
merged: true
base: main
head: better-error-message-on-pyproject-toml-parsing
created_at: 2024-08-11T15:32:09Z
updated_at: 2024-08-11T21:13:14Z
url: https://github.com/astral-sh/uv/pull/6009
synced_at: 2026-01-12T16:07:09Z
```

# Make more informative warning message when failed to parse pyproject.toml

---

_@flyaroundme_

## Summary

Added the actual error message to the warning when uv fails to parse `pyproject.toml`.

Resolves https://github.com/astral-sh/uv/issues/5934

## Test Plan

Took the case from the issue:
- have `pyproject.toml` which contains
```
[tool.uv]
foobar = false
```
- 
```
$ uv venv --preview -v
```
- Expect the message that contains the actual problem in the `pyproject.toml` like:
```
warning: Failed to parse `pyproject.toml` during settings discovery: unknown field `foobar`; skipping...
```

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-11 19:23_

---

_Label `error messages` added by @charliermarsh on 2024-08-11 19:24_

---

_@charliermarsh approved on 2024-08-11 21:07_

Thanks, good idea. I just indented the error.

---

_Merged by @charliermarsh on 2024-08-11 21:13_

---

_Closed by @charliermarsh on 2024-08-11 21:13_

---
