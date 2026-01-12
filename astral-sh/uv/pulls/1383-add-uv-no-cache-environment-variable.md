```yaml
number: 1383
title: "Add `UV_NO_CACHE` environment variable"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/no-cache
created_at: 2024-02-15T23:27:00Z
updated_at: 2024-02-15T23:34:43Z
url: https://github.com/astral-sh/uv/pull/1383
synced_at: 2026-01-12T16:04:36Z
```

# Add `UV_NO_CACHE` environment variable

---

_@zanieb_

It's a little picky about the value, but that seems okay.

```
❯ ./target/debug/uv pip install trio
Audited 1 package in 4ms
❯ UV_NO_CACHE=true ./target/debug/uv pip install trio
Audited 1 package in 50ms
```

Closes #1382 

---

_Label `enhancement` added by @zanieb on 2024-02-15 23:28_

---

_@charliermarsh approved on 2024-02-15 23:31_

---

_Merged by @zanieb on 2024-02-15 23:34_

---

_Closed by @zanieb on 2024-02-15 23:34_

---

_Branch deleted on 2024-02-15 23:34_

---
