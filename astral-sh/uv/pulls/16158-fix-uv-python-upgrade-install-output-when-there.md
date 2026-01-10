```yaml
number: 16158
title: "Fix `uv python upgrade / install` output when there is a no-op for one request"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-output-install
created_at: 2025-10-07T17:27:16Z
updated_at: 2025-10-07T18:39:30Z
url: https://github.com/astral-sh/uv/pull/16158
synced_at: 2026-01-10T06:36:15Z
```

# Fix `uv python upgrade / install` output when there is a no-op for one request

---

_Pull request opened by @zanieb on 2025-10-07 17:27_

_No description provided._

---

_Label `bug` added by @zanieb on 2025-10-07 17:27_

---

_Marked ready for review by @zanieb on 2025-10-07 18:24_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:565 on 2025-10-07 18:27_

```suggestion
                    "{request} is already on the latest supported patch release"
```


---

_Review comment by @zanieb on `crates/uv/tests/it/python_upgrade.rs`:54 on 2025-10-07 18:28_

```suggestion
    Python 3.10 is already on the latest supported patch release
```


---

_@zanieb reviewed on 2025-10-07 18:28_

---

_Merged by @zanieb on 2025-10-07 18:39_

---

_Closed by @zanieb on 2025-10-07 18:39_

---

_Branch deleted on 2025-10-07 18:39_

---
