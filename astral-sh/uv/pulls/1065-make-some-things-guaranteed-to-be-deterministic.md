```yaml
number: 1065
title: make some things guaranteed to be deterministic
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/non-deterministic-test
created_at: 2024-01-23T16:28:38Z
updated_at: 2024-01-24T01:30:34Z
url: https://github.com/astral-sh/uv/pull/1065
synced_at: 2026-01-12T16:04:23Z
```

# make some things guaranteed to be deterministic

---

_@BurntSushi_

This PR replaces a few uses of hash maps/sets with btree maps/sets and
index maps/sets. This has the benefit of guaranteeing a deterministic
order of iteration.

I made these changes as part of looking into a flaky test.
Unfortunately, I'm not optimistic that anything here will actually fix
the flaky test, since I don't believe anything was actually dependent
on the order of iteration.


---

_Review requested from @charliermarsh by @BurntSushi on 2024-01-23 16:28_

---

_Review requested from @zanieb by @BurntSushi on 2024-01-23 16:28_

---

_@zanieb approved on 2024-01-23 21:42_

---

_Merged by @BurntSushi on 2024-01-24 01:30_

---

_Closed by @BurntSushi on 2024-01-24 01:30_

---

_Branch deleted on 2024-01-24 01:30_

---
