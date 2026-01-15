```yaml
number: 17488
title: Log when response arrives
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/log-reponse
created_at: 2026-01-15T16:53:09Z
updated_at: 2026-01-15T17:23:53Z
url: https://github.com/astral-sh/uv/pull/17488
synced_at: 2026-01-15T17:50:37Z
```

# Log when response arrives

---

_@konstin_

Looking at #17485, I noticed we don't log when a request finishes, making the log harder to read than necessary.

Example new log message:

```
TRACE Received response for revalidation request with status 304 Not Modified for: https://pypi.org/simple/idna/
```

---

_Label `internal` added by @konstin on 2026-01-15 16:53_

---

_@konstin reviewed on 2026-01-15 16:53_

---

_Review comment by @konstin on `crates/uv-client/src/cached_client.rs`:612 on 2026-01-15 16:53_

To match the format of the revalidation branch

---

_@zanieb approved on 2026-01-15 17:08_

---

_Merged by @konstin on 2026-01-15 17:23_

---

_Closed by @konstin on 2026-01-15 17:23_

---

_Branch deleted on 2026-01-15 17:23_

---
