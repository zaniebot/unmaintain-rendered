```yaml
number: 5545
title: Retain editable designation for cached wheel installs
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ed
created_at: 2024-07-29T02:32:37Z
updated_at: 2024-07-29T02:39:49Z
url: https://github.com/astral-sh/uv/pull/5545
synced_at: 2026-01-12T16:06:53Z
```

# Retain editable designation for cached wheel installs

---

_@charliermarsh_

## Summary

The package was being installed as editable, but it wasn't marked as such in `uv pip list`, as the `direct-url.json` was wrong.

Closes https://github.com/astral-sh/uv/issues/5543.


---

_Label `bug` added by @charliermarsh on 2024-07-29 02:32_

---

_Merged by @charliermarsh on 2024-07-29 02:39_

---

_Closed by @charliermarsh on 2024-07-29 02:39_

---

_Branch deleted on 2024-07-29 02:39_

---
