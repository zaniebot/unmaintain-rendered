```yaml
number: 8256
title: Respect index priority when storing credentials
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ord
created_at: 2024-10-16T14:02:10Z
updated_at: 2024-10-16T15:52:27Z
url: https://github.com/astral-sh/uv/pull/8256
synced_at: 2026-01-10T12:54:05Z
```

# Respect index priority when storing credentials

---

_Pull request opened by @charliermarsh on 2024-10-16 14:02_

## Summary

Closes https://github.com/astral-sh/uv/issues/8248.


---

_Label `bug` added by @charliermarsh on 2024-10-16 14:02_

---

_Comment by @zanieb on 2024-10-16 14:20_

Interesting. Another case for something like #4583 

---

_@zanieb approved on 2024-10-16 14:24_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/compile.rs`:295 on 2024-10-16 14:25_

Maybe we should have some sort of `uv_auth::store_credentials` method that takes `index_locations` directly so we can avoid repeating this pattern everywhere and document why it's reversed and such?

---

_@zanieb reviewed on 2024-10-16 14:25_

---

_@charliermarsh reviewed on 2024-10-16 14:47_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/compile.rs`:295 on 2024-10-16 14:47_

I'd prefer not to couple them for now. `uv_auth` has no dependencies as-is.

---

_Merged by @charliermarsh on 2024-10-16 15:52_

---

_Closed by @charliermarsh on 2024-10-16 15:52_

---

_Branch deleted on 2024-10-16 15:52_

---
