```yaml
number: 13995
title: Store distributions in cache with content addressed keys
type: issue
state: open
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2025-06-12T13:56:26Z
updated_at: 2025-06-12T14:34:40Z
url: https://github.com/astral-sh/uv/issues/13995
synced_at: 2026-01-12T16:01:41Z
```

# Store distributions in cache with content addressed keys

---

_@zanieb_

e.g., in https://github.com/astral-sh/uv/issues/12480#issuecomment-2966772503 we store duplicate cache entries for a wheel across multiple indexes because we do not eagerly compute hashes for all distributions and cannot store them in a content addressed key.

Since it's just a file, we can trivially hash them — we'll want to be careful about performance and consider the hashing algorithm though, like ideally we'd use Blake2 but the lockfile requires SHA-256 and it may not be worth hashing twice?

---

_Label `enhancement` added by @konstin on 2025-06-12 14:34_

---
