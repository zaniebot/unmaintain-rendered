```yaml
number: 713
title: Update pubgrub
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/update-pubgrub
created_at: 2023-12-20T13:39:58Z
updated_at: 2023-12-20T22:57:00Z
url: https://github.com/astral-sh/uv/pull/713
synced_at: 2026-01-12T16:04:08Z
```

# Update pubgrub

---

_@konstin_

Easier than i expected: We simply never construct the pubgrub error variants since we have our own main loop. The `unreachable!()`s can be removed when never is stabilized

---

_@zanieb reviewed on 2023-12-20 15:46_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/error.rs`:120 on 2023-12-20 15:46_

We should remove these `ResolveError` variants, right?

---

_Review requested from @zanieb by @konstin on 2023-12-20 22:10_

---

_@zanieb approved on 2023-12-20 22:35_

---

_Merged by @konstin on 2023-12-20 22:56_

---

_Closed by @konstin on 2023-12-20 22:57_

---

_Branch deleted on 2023-12-20 22:57_

---
