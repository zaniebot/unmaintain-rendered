```yaml
number: 7925
title: Write timout for publishing
type: issue
state: open
author: konstin
labels:
  - enhancement
assignees: []
created_at: 2024-10-04T13:51:54Z
updated_at: 2024-10-04T13:51:54Z
url: https://github.com/astral-sh/uv/issues/7925
synced_at: 2026-01-12T15:59:17Z
```

# Write timout for publishing

---

_@konstin_

Currently, we use a high read timeout for reqwest in uploads (#7923) since reqwest is lacking a dedicated write timeout option (https://github.com/seanmonstar/reqwest/issues/2403). Instead, this should be properly solved by a write timeout (also 30s) and no total limit on the upload time.

---

_Label `enhancement` added by @konstin on 2024-10-04 13:51_

---
