```yaml
number: 10662
title: Use 32-core Windows runner for tests
type: pull_request
state: closed
author: zanieb
labels:
  - internal
assignees: []
draft: true
base: main
head: zb/win-32
created_at: 2025-01-15T23:40:10Z
updated_at: 2025-04-21T20:06:16Z
url: https://github.com/astral-sh/uv/pull/10662
synced_at: 2026-01-10T11:10:34Z
```

# Use 32-core Windows runner for tests

---

_Pull request opened by @zanieb on 2025-01-15 23:40_

Historically, this has not been worth it; worth a try though

- 73cade13862c88bf2d5344759e9b1b7035b63743 (latest `main`): 7m 39s (test) / 9m 24s (total)
- 8325fae996c513d46976bede99761ce2a64735e4 (based on `main`): 6m 18s / 8m 11s
- e12e35f1de4493bb586c7dee93f66d8432002c40 (based on other dev-drive improvements): 5m 47s / 6m 59s

Something like a 25% improvement?

---

_Label `internal` added by @zanieb on 2025-01-15 23:40_

---

_Comment by @zanieb on 2025-01-24 19:31_

Now on the latest `main` it's 7m 32s / 5m 32s vs 5m 57s / 4m 22s (total / test)


---

_Comment by @zanieb on 2025-04-21 20:05_

Now 8m 1s -> 6m 49s (total). About the same, not clear it's worth the spend.

---

_Closed by @zanieb on 2025-04-21 20:05_

---
