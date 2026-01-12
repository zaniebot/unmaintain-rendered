```yaml
number: 6864
title: "Add warning when `VIRTUAL_ENV` is set but will not be respected in project commands"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/project-venv-warn
created_at: 2024-08-30T13:05:36Z
updated_at: 2024-09-04T02:59:54Z
url: https://github.com/astral-sh/uv/pull/6864
synced_at: 2026-01-12T16:07:33Z
```

# Add warning when `VIRTUAL_ENV` is set but will not be respected in project commands

---

_@zanieb_

Following https://github.com/astral-sh/uv/pull/6834

---

_Label `cli` added by @zanieb on 2024-08-30 13:05_

---

_Review requested from @charliermarsh by @zanieb on 2024-09-03 20:45_

---

_Review requested from @BurntSushi by @zanieb on 2024-09-03 20:45_

---

_@zanieb reviewed on 2024-09-03 20:48_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:487 on 2024-09-03 20:48_

A little awkward, but important for cases where `.venv` is being created in a project — it'd be weird not to warn on the first invocation but warn on later ones.

---

_@MithicSpirit reviewed on 2024-09-03 21:26_

---

_Review comment by @MithicSpirit on `crates/uv-workspace/src/workspace.rs`:487 on 2024-09-03 21:26_

Is there any value to the previous check when this one's already here? This just feels like a more general version of the previous one. The only thing I can think of is that this one will trigger on case-insensitive filesystems if the casing differs, while I don't think that `is_same_file` will.

---

_Merged by @charliermarsh on 2024-09-03 23:51_

---

_Closed by @charliermarsh on 2024-09-03 23:51_

---

_Branch deleted on 2024-09-03 23:51_

---

_@zanieb reviewed on 2024-09-04 02:59_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:487 on 2024-09-04 02:59_

I'm not sure honestly. Seems safe to keep it.

cc @BurntSushi for a post-merge review if you have time.

---
