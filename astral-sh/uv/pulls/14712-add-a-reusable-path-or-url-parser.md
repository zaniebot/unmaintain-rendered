```yaml
number: 14712
title: Add a reusable path-or-URL parser
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/generic-path-or-url
created_at: 2025-07-18T11:04:55Z
updated_at: 2025-07-18T12:08:50Z
url: https://github.com/astral-sh/uv/pull/14712
synced_at: 2026-01-12T16:11:21Z
```

# Add a reusable path-or-URL parser

---

_@konstin_

Reviewing #14687, I noticed that we had implemented a `Url::from_url_or_path`-like function, but it wasn't reusable. This change `Verbatim::from_url_or_path` so we can use it in other places too.

The PEP 508 parser is an odd place for this, but that's where `VerbatimUrl` and `Scheme` are already living.


---

_Label `internal` added by @konstin on 2025-07-18 11:05_

---

_@zanieb reviewed on 2025-07-18 11:39_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index_url.rs`:40 on 2025-07-18 11:39_

Can you copy this comment too?

---

_@zanieb approved on 2025-07-18 11:39_

---

_@zanieb approved on 2025-07-18 12:01_

---

_Merged by @konstin on 2025-07-18 12:08_

---

_Closed by @konstin on 2025-07-18 12:08_

---

_Branch deleted on 2025-07-18 12:08_

---
