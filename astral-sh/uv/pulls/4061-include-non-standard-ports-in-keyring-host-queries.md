```yaml
number: 4061
title: Include non-standard ports in keyring host queries
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/keyring
created_at: 2024-06-05T18:29:29Z
updated_at: 2024-06-07T00:02:48Z
url: https://github.com/astral-sh/uv/pull/4061
synced_at: 2026-01-10T13:54:02Z
```

# Include non-standard ports in keyring host queries

---

_Pull request opened by @zanieb on 2024-06-05 18:29_

Partially addresses https://github.com/astral-sh/uv/issues/4056

We were incorrectly omitting the port from requests to `keyring` when falling back to a realm/host query, e.g. `localhost` was used instead of `localhost:1234`. We still won't include "standard" ports like `80` for an HTTP request.

---

_Label `bug` added by @zanieb on 2024-06-05 18:29_

---

_Marked ready for review by @zanieb on 2024-06-06 18:52_

---

_@charliermarsh approved on 2024-06-07 00:01_

---

_Merged by @zanieb on 2024-06-07 00:02_

---

_Closed by @zanieb on 2024-06-07 00:02_

---

_Branch deleted on 2024-06-07 00:02_

---
