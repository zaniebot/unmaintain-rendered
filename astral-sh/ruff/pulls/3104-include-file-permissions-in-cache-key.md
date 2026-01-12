```yaml
number: 3104
title: Include file permissions in cache key
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ctime
created_at: 2023-02-21T22:43:19Z
updated_at: 2023-02-21T23:20:08Z
url: https://github.com/astral-sh/ruff/pull/3104
synced_at: 2026-01-12T04:39:44Z
```

# Include file permissions in cache key

---

_Pull request opened by @charliermarsh on 2023-02-21 22:43_

Closes #3086.

---

_Label `bug` added by @charliermarsh on 2023-02-21 22:43_

---

_@charliermarsh reviewed on 2023-02-21 22:43_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/cache.rs`:47 on 2023-02-21 22:43_

We now include the timestamp in the cache key. This should be more performant, since we don't have to deserialize from the cache to know that it's a miss.

---

_Merged by @charliermarsh on 2023-02-21 23:20_

---

_Closed by @charliermarsh on 2023-02-21 23:20_

---

_Branch deleted on 2023-02-21 23:20_

---
