---
number: 5939
title: "Add test coverage for `uv python` using proxy to serve dummy distributions"
type: issue
state: open
author: zanieb
labels:
  - help wanted
  - testing
assignees: []
created_at: 2024-08-08T21:42:03Z
updated_at: 2024-11-04T12:31:39Z
url: https://github.com/astral-sh/uv/issues/5939
synced_at: 2026-01-10T01:23:54Z
---

# Add test coverage for `uv python` using proxy to serve dummy distributions

---

_Issue opened by @zanieb on 2024-08-08 21:42_

Actually installing distributions is too expensive for unit tests, but now that we have #5719 we can host dummy distributions (i.e. only with necessary stub files) in a proxy and write unit tests for `uv python install` and friends.

---

_Label `help wanted` added by @zanieb on 2024-08-08 21:42_

---

_Label `testing` added by @zanieb on 2024-08-08 21:42_

---

_Referenced in [astral-sh/uv#8662](../../astral-sh/uv/issues/8662.md) on 2024-11-04 12:31_

---

_Comment by @zanieb on 2024-11-04 12:31_

(We could also even host the real distributions from a local cache as a first step to improve the test speed)

---
