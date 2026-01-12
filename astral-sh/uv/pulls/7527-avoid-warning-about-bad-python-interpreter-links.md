```yaml
number: 7527
title: Avoid warning about bad Python interpreter links for empty project environment directories
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/fix-bad-link-warning
created_at: 2024-09-19T01:02:26Z
updated_at: 2024-09-19T11:49:36Z
url: https://github.com/astral-sh/uv/pull/7527
synced_at: 2026-01-12T16:07:52Z
```

# Avoid warning about bad Python interpreter links for empty project environment directories

---

_@zanieb_

Someone reported this a while back (will try to find the issue), and I ran into it working on #7522 

---

_Renamed from "Avoid displaying warning about bad Python intepreter links for empty project virtual environment directories" to "Avoid warning about bad Python interpreter links for empty project environment directories" by @zanieb on 2024-09-19 01:03_

---

_@zanieb reviewed on 2024-09-19 01:07_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:403 on 2024-09-19 01:07_

This change suggests #7522 might not be quite right, maybe its check should be here?

---

_Review requested from @charliermarsh by @zanieb on 2024-09-19 01:07_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:403 on 2024-09-19 01:28_

I think it makes sense to put the logic here.

---

_@charliermarsh reviewed on 2024-09-19 01:28_

---

_@charliermarsh approved on 2024-09-19 01:28_

---

_@zanieb reviewed on 2024-09-19 01:35_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:403 on 2024-09-19 01:35_

It seemed nice to have it close to the deletion logic so we know that we're not just deleting a random directory, but yeah this probably makes more sense. Will do.

---

_Label `cli` added by @zanieb on 2024-09-19 02:27_

---

_Merged by @zanieb on 2024-09-19 11:49_

---

_Closed by @zanieb on 2024-09-19 11:49_

---

_Branch deleted on 2024-09-19 11:49_

---
