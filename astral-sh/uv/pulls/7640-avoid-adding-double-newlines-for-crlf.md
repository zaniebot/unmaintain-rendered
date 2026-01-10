```yaml
number: 7640
title: Avoid adding double-newlines for CRLF
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/cr
created_at: 2024-09-23T13:51:21Z
updated_at: 2024-09-23T13:58:25Z
url: https://github.com/astral-sh/uv/pull/7640
synced_at: 2026-01-10T12:53:52Z
```

# Avoid adding double-newlines for CRLF

---

_Pull request opened by @charliermarsh on 2024-09-23 13:51_

## Summary

Closes https://github.com/astral-sh/uv/issues/7621.

## Test Plan

Before:

- `uv init`
- `uv add flask`
- Change to CRLF in the editor
- `uv add httpx` (leads to double newlines)

After:

- `uv init`
- `uv add flask`
- Change to CRLF in the editor
- `cargo run add httpx` (leads to correct newlines)


---

_Label `bug` added by @charliermarsh on 2024-09-23 13:51_

---

_Label `windows` added by @charliermarsh on 2024-09-23 13:51_

---

_Merged by @charliermarsh on 2024-09-23 13:58_

---

_Closed by @charliermarsh on 2024-09-23 13:58_

---

_Branch deleted on 2024-09-23 13:58_

---
