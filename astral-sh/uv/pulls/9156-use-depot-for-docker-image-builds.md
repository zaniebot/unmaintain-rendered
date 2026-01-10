```yaml
number: 9156
title: Use Depot for Docker image builds
type: pull_request
state: open
author: zanieb
labels:
  - internal
assignees: []
draft: true
base: main
head: zb/depot-docker
created_at: 2024-11-15T22:31:38Z
updated_at: 2025-04-14T08:00:32Z
url: https://github.com/astral-sh/uv/pull/9156
synced_at: 2026-01-10T11:10:34Z
```

# Use Depot for Docker image builds

---

_Pull request opened by @zanieb on 2024-11-15 22:31_

Exploring the experience here

---

_Label `internal` added by @zanieb on 2024-11-15 22:31_

---

_Comment by @zanieb on 2024-11-15 23:30_

It looks like there's ~no speed increase. I think we'd need to change our caching somehow?

---

_Comment by @samypr100 on 2024-11-23 01:43_

> It looks like there's ~no speed increase. I think we'd need to change our caching somehow?

Yea, try commenting out the `cache-to` and `cache-from` lines. Depot's [action ](https://github.com/depot/build-push-action) suggests to remove them.

---
