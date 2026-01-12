```yaml
number: 8019
title: Update rust-cache in CI to be shared across jobs
type: pull_request
state: closed
author: zanieb
labels:
  - internal
assignees: []
draft: true
base: main
head: zanie/shared-cache
created_at: 2023-10-17T16:02:57Z
updated_at: 2023-10-18T02:08:05Z
url: https://github.com/astral-sh/ruff/pull/8019
synced_at: 2026-01-12T15:55:25Z
```

# Update rust-cache in CI to be shared across jobs

---

_@zanieb_

By default, the cache key includes the job identifier so we have a separate cache for each of our CI builds


---

_Label `internal` added by @zanieb on 2023-10-17 16:02_

---

_Comment by @zanieb on 2023-10-17 16:03_

Checking if this has any benefit

---

_Comment by @MichaReiser on 2023-10-17 23:04_

It's unclear to me if this helps because each job (or most) have different build targets (test, vs debug, vs dependencies). But maybe it does. 

---

_Comment by @zanieb on 2023-10-18 02:08_

Yeah I think it's not effective like this.

---

_Closed by @zanieb on 2023-10-18 02:08_

---
