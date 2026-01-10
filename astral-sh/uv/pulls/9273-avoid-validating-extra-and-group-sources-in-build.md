```yaml
number: 9273
title: "Avoid validating extra and group sources in `build-system.requires`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/build-requires
created_at: 2024-11-20T13:47:36Z
updated_at: 2024-11-20T14:05:53Z
url: https://github.com/astral-sh/uv/pull/9273
synced_at: 2026-01-10T12:00:00Z
```

# Avoid validating extra and group sources in `build-system.requires`

---

_Pull request opened by @charliermarsh on 2024-11-20 13:47_

## Summary

This was an oversight in the initial implementation. We shouldn't validate sources for the `build-system.requires` field, since extras and groups can _never_ be active.

Closes https://github.com/astral-sh/uv/issues/9259.


---

_Label `bug` added by @charliermarsh on 2024-11-20 13:47_

---

_Merged by @charliermarsh on 2024-11-20 14:05_

---

_Closed by @charliermarsh on 2024-11-20 14:05_

---

_Branch deleted on 2024-11-20 14:05_

---
