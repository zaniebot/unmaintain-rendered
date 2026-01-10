```yaml
number: 10462
title: failover on multiple indexes
type: issue
state: open
author: yuzhichang
labels:
  - wish
  - needs-decision
assignees: []
created_at: 2025-01-10T08:53:51Z
updated_at: 2025-01-13T02:42:23Z
url: https://github.com/astral-sh/uv/issues/10462
synced_at: 2026-01-10T04:27:58Z
```

# failover on multiple indexes

---

_Issue opened by @yuzhichang on 2025-01-10 08:53_

My scenario is:
The access to pypi.org is slow, or even broken very often. There are some public pypi mirrors here but none is stable (reachable, fast, can serve query and downloading) enough.  These mirrors are hosted by famous companies or universities so I trust them.
I want "uv pip install" to pick the fastest and up-to-date one among them. If it's broken (reachable but cannot serve query and downloading for some unknown reason), the try another one.

As far as I know, neither pip nor uv  failovers on multiple indexes. This forced me to change index from time to time. This is annoying.


---

_Label `wish` added by @charliermarsh on 2025-01-13 02:42_

---

_Label `needs-decision` added by @charliermarsh on 2025-01-13 02:42_

---
