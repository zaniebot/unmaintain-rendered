```yaml
number: 3726
title: "Add `StateStore` abstraction for persistent state storage"
type: pull_request
state: closed
author: zanieb
labels:
  - internal
assignees: []
draft: true
base: main
head: zb/data-dir
created_at: 2024-05-21T22:58:23Z
updated_at: 2024-05-24T15:19:49Z
url: https://github.com/astral-sh/uv/pull/3726
synced_at: 2026-01-12T16:05:49Z
```

# Add `StateStore` abstraction for persistent state storage

---

_@zanieb_

In preparation for storing stateful data that is not considered cache or config e.g.

- `uv tool install` environments
- `uv toolchain` downloads

Cribbed from the `uv-cache::Cache` interface.

---

_Label `internal` added by @zanieb on 2024-05-21 22:58_

---

_Comment by @zanieb on 2024-05-21 23:40_

Building toolchain storage on top of this before it's ready for a look.

---

_Comment by @zanieb on 2024-05-24 15:19_

Rolling into #3797 

---

_Closed by @zanieb on 2024-05-24 15:19_

---
