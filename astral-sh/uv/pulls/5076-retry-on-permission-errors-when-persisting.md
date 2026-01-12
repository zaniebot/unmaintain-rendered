```yaml
number: 5076
title: Retry on permission errors when persisting extracted source distributions to the cache
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/retry-cache-windows
created_at: 2024-07-15T16:00:38Z
updated_at: 2024-07-15T17:56:09Z
url: https://github.com/astral-sh/uv/pull/5076
synced_at: 2026-01-12T16:06:37Z
```

# Retry on permission errors when persisting extracted source distributions to the cache

---

_@zanieb_

Another case for https://github.com/astral-sh/uv/issues/1491

ref #4606 

---

_Label `bug` added by @zanieb on 2024-07-15 16:00_

---

_Comment by @zanieb on 2024-07-15 16:02_

I believe this is actually the last remaining case of `fs_err::tokio::rename`. As previously noted, there are also synchronous `rename` calls that we do not retry on yet.

---

_Review requested from @charliermarsh by @zanieb on 2024-07-15 16:04_

---

_Review requested from @BurntSushi by @zanieb on 2024-07-15 16:20_

---

_@BurntSushi approved on 2024-07-15 16:57_

---

_Merged by @zanieb on 2024-07-15 17:56_

---

_Closed by @zanieb on 2024-07-15 17:56_

---

_Branch deleted on 2024-07-15 17:56_

---
