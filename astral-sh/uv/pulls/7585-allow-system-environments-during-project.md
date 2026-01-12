```yaml
number: 7585
title: Allow system environments during project environment validity check
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-project-system
created_at: 2024-09-20T14:14:11Z
updated_at: 2024-09-20T17:28:20Z
url: https://github.com/astral-sh/uv/pull/7585
synced_at: 2026-01-12T16:07:53Z
```

# Allow system environments during project environment validity check

---

_@zanieb_

_No description provided._

---

_Label `bug` added by @zanieb on 2024-09-20 14:14_

---

_@bluss reviewed on 2024-09-20 14:40_

---

_Review comment by @bluss on `crates/uv-python/src/environment.rs`:191 on 2024-09-20 14:40_

Would this mean that a ~/ where ~/bin/python exists is an environment?

---

_Review comment by @zanieb on `crates/uv-python/src/environment.rs`:191 on 2024-09-20 14:42_

Uhh yeah. Which could be problematic yes. And `/` where `/bin/python` exists. 

Unfortunately I can't think of a better way to tell if an arbitrary directory is a system Python environment?

---

_@zanieb reviewed on 2024-09-20 14:42_

---

_@zanieb reviewed on 2024-09-20 16:21_

---

_Review comment by @zanieb on `crates/uv-python/src/environment.rs`:191 on 2024-09-20 16:21_

Okay I think I've guarded against this now by only deleting virtual environments.

---

_@charliermarsh approved on 2024-09-20 17:09_

---

_Merged by @zanieb on 2024-09-20 17:28_

---

_Closed by @zanieb on 2024-09-20 17:28_

---

_Branch deleted on 2024-09-20 17:28_

---
