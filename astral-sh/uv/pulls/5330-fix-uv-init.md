```yaml
number: 5330
title: "Fix `uv init .`"
type: pull_request
state: merged
author: eth3lbert
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: init-dot
created_at: 2024-07-23T06:08:08Z
updated_at: 2024-07-23T12:26:16Z
url: https://github.com/astral-sh/uv/pull/5330
synced_at: 2026-01-12T16:06:45Z
```

# Fix `uv init .`

---

_@eth3lbert_

## Summary

This PR avoids an `Invalid package name` error that occurs when using `uv init .`. This is achieved by slightly reorganizing the code block to determine the name after the path is canonicalized. The dot path is expanded to the current directory, and the `file_name` then works as expected.

Resolve #5329 .




---

_@konstin approved on 2024-07-23 08:36_

---

_Label `bug` added by @konstin on 2024-07-23 08:36_

---

_Merged by @charliermarsh on 2024-07-23 12:20_

---

_Closed by @charliermarsh on 2024-07-23 12:20_

---

_Label `preview` added by @charliermarsh on 2024-07-23 12:20_

---

_Branch deleted on 2024-07-23 12:26_

---
