```yaml
number: 4471
title: Add context to unregistered task name to error context
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/unregistered-context
created_at: 2024-06-24T13:19:05Z
updated_at: 2024-06-24T14:42:57Z
url: https://github.com/astral-sh/uv/pull/4471
synced_at: 2026-01-12T16:06:15Z
```

# Add context to unregistered task name to error context

---

_@konstin_

I caused this error during development and having the name of the task on it is helpful for debugging.

Split out from #4435

---

_Label `internal` added by @konstin on 2024-06-24 13:19_

---

_@charliermarsh approved on 2024-06-24 13:23_

---

_Comment by @charliermarsh on 2024-06-24 13:25_

Is the dist ID enough to distinguish the "task"?

---

_Comment by @charliermarsh on 2024-06-24 13:25_

I have also done something like this so very much in favor.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:999 on 2024-06-24 13:25_

Should these now be in a closure to avoid allocation?

---

_@charliermarsh reviewed on 2024-06-24 13:25_

---

_Comment by @konstin on 2024-06-24 13:27_

> Is the dist ID enough to distinguish the "task"?

For me the problem was package name vs. url so this was sufficient

---

_@konstin reviewed on 2024-06-24 13:27_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:999 on 2024-06-24 13:27_

Good idea, will change

---

_Merged by @konstin on 2024-06-24 14:42_

---

_Closed by @konstin on 2024-06-24 14:42_

---

_Branch deleted on 2024-06-24 14:42_

---
