```yaml
number: 3243
title: "Remove `KeyringProvider.cache`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/keyring-cache
created_at: 2024-04-24T15:17:11Z
updated_at: 2024-04-24T15:39:25Z
url: https://github.com/astral-sh/uv/pull/3243
synced_at: 2026-01-12T16:05:31Z
```

# Remove `KeyringProvider.cache`

---

_@zanieb_

This is handled by `CredentialsCache.fetches` instead since #3237 

Moves the test demonstrating the flaw in the cache to the middleware level.


---

_Label `internal` added by @zanieb on 2024-04-24 15:17_

---

_Review requested from @charliermarsh by @zanieb on 2024-04-24 15:23_

---

_@charliermarsh approved on 2024-04-24 15:24_

---

_Merged by @zanieb on 2024-04-24 15:39_

---

_Closed by @zanieb on 2024-04-24 15:39_

---

_Branch deleted on 2024-04-24 15:39_

---
