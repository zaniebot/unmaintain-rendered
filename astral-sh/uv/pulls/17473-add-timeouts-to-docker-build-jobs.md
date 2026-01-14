```yaml
number: 17473
title: Add timeouts to Docker build jobs
type: pull_request
state: open
author: zanieb
labels:
  - internal
assignees: []
base: main
head: zb/timeout-docker
created_at: 2026-01-14T18:31:07Z
updated_at: 2026-01-14T18:32:26Z
url: https://github.com/astral-sh/uv/pull/17473
synced_at: 2026-01-14T18:48:19Z
```

# Add timeouts to Docker build jobs

---

_@zanieb_

These are hitting OIDC failures on Depot and run for 30m consuming a lot of CI concurrency.

These timeouts are based on historical data retrieved by Claude.

---

_Label `internal` added by @zanieb on 2026-01-14 18:31_

---

_Marked ready for review by @zanieb on 2026-01-14 18:32_

---
