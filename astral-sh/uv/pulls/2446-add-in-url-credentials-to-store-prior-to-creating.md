```yaml
number: 2446
title: Add in-URL credentials to store prior to creating requests
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/auth
created_at: 2024-03-14T03:23:14Z
updated_at: 2024-03-14T03:46:34Z
url: https://github.com/astral-sh/uv/pull/2446
synced_at: 2026-01-12T16:05:03Z
```

# Add in-URL credentials to store prior to creating requests

---

_@charliermarsh_

## Summary

The authentication middleware extracts in-URL credentials from URLs that pass through it; however, by the time a request reaches the store, the credentials will have already been removed, and relocated to the header. So we were never propagating in-URL credentials.

This PR adds an explicit pass wherein we pass in-URL credentials to the store prior to doing any work.

Closes https://github.com/astral-sh/uv/issues/2444.

## Test Plan

`cargo run pip install` against an authenticated AWS registry.


---

_Review requested from @zanieb by @charliermarsh on 2024-03-14 03:23_

---

_Label `bug` added by @charliermarsh on 2024-03-14 03:23_

---

_@zanieb approved on 2024-03-14 03:25_

Thanks! I'll investigate other approaches.

---

_Merged by @charliermarsh on 2024-03-14 03:46_

---

_Closed by @charliermarsh on 2024-03-14 03:46_

---

_Branch deleted on 2024-03-14 03:46_

---
