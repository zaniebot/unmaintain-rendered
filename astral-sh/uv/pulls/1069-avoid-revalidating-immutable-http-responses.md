```yaml
number: 1069
title: Avoid revalidating immutable HTTP responses
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/reval
created_at: 2024-01-23T21:16:49Z
updated_at: 2024-01-23T21:22:22Z
url: https://github.com/astral-sh/uv/pull/1069
synced_at: 2026-01-12T16:04:24Z
```

# Avoid revalidating immutable HTTP responses

---

_@charliermarsh_

## Summary

If you send a revalidation request to a resource that returns an `immutable` directive, the server apparently returns a 200 instead of a 304?  In other words, the server can ignore the revalidation request. This PR adds handling on top of the HTTP cache semantics to respect immutable resources, which is especially useful since all PyPI files are immutable.


---

_Label `bug` added by @charliermarsh on 2024-01-23 21:16_

---

_Merged by @charliermarsh on 2024-01-23 21:22_

---

_Closed by @charliermarsh on 2024-01-23 21:22_

---

_Branch deleted on 2024-01-23 21:22_

---
