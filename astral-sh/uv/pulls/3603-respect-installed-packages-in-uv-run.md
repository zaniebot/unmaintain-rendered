```yaml
number: 3603
title: "Respect installed packages in `uv run`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/inst
created_at: 2024-05-15T14:34:07Z
updated_at: 2024-05-15T15:48:33Z
url: https://github.com/astral-sh/uv/pull/3603
synced_at: 2026-01-12T16:05:44Z
```

# Respect installed packages in `uv run`

---

_@charliermarsh_

Closes #3601.

---

_Label `bug` added by @charliermarsh on 2024-05-15 14:34_

---

_Label `preview` added by @charliermarsh on 2024-05-15 14:34_

---

_Marked ready for review by @charliermarsh on 2024-05-15 14:34_

---

_Review requested from @zanieb by @charliermarsh on 2024-05-15 14:34_

---

_@zanieb reviewed on 2024-05-15 15:23_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/lock.rs`:45 on 2024-05-15 15:23_

Should locking prefer installed packages? We don't do this in `pip compile`.

---

_@zanieb approved on 2024-05-15 15:23_

---

_@charliermarsh reviewed on 2024-05-15 15:29_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:45 on 2024-05-15 15:29_

Not sure...

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:45 on 2024-05-15 15:43_

Removed.

---

_@charliermarsh reviewed on 2024-05-15 15:43_

---

_Merged by @charliermarsh on 2024-05-15 15:48_

---

_Closed by @charliermarsh on 2024-05-15 15:48_

---

_Branch deleted on 2024-05-15 15:48_

---
