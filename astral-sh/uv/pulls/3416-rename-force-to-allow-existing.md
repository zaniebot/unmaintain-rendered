```yaml
number: 3416
title: "Rename `--force` to `--allow-existing`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/force
created_at: 2024-05-07T04:29:25Z
updated_at: 2024-05-07T17:11:32Z
url: https://github.com/astral-sh/uv/pull/3416
synced_at: 2026-01-10T14:37:54Z
```

# Rename `--force` to `--allow-existing`

---

_Pull request opened by @charliermarsh on 2024-05-07 04:29_

## Summary

We've found `--force` to be a confusing name here. (Not breaking as this hasn't shipped in a release.)


---

_Review requested from @zanieb by @charliermarsh on 2024-05-07 04:30_

---

_Label `cli` added by @charliermarsh on 2024-05-07 04:30_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/lib.rs`:101 on 2024-05-07 04:30_

(Not related, but these comments are wrong. This was just dirty on my branch.)

---

_@charliermarsh reviewed on 2024-05-07 04:30_

---

_@zanieb reviewed on 2024-05-07 13:22_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:1713 on 2024-05-07 13:22_

Should we say like `--unsafe-allow-existing` since it could bork the environment? We could make it a safe flag in the future with checks for compatible interpreter versions?

---

_@zanieb approved on 2024-05-07 13:23_

---

_@zanieb reviewed on 2024-05-07 13:23_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:1713 on 2024-05-07 13:23_

Do we need to document the caveats of how this could break an environment?

---

_@charliermarsh reviewed on 2024-05-07 13:52_

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:1713 on 2024-05-07 13:52_

I'll document it. It should _generally_ be fine since the virtual environment at least stores packages under their own minor Python version? virtualenv already does this today as its default behavior.

---

_Merged by @charliermarsh on 2024-05-07 17:11_

---

_Closed by @charliermarsh on 2024-05-07 17:11_

---

_Branch deleted on 2024-05-07 17:11_

---
