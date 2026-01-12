```yaml
number: 2480
title: "Tweak `uv pip check` output for consistency with other interfaces"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/check
created_at: 2024-03-15T18:21:09Z
updated_at: 2024-03-18T17:49:12Z
url: https://github.com/astral-sh/uv/pull/2480
synced_at: 2026-01-12T16:05:04Z
```

# Tweak `uv pip check` output for consistency with other interfaces

---

_@zanieb_

_No description provided._

---

_Label `internal` added by @zanieb on 2024-03-15 18:21_

---

_Marked ready for review by @zanieb on 2024-03-15 19:35_

---

_@zanieb reviewed on 2024-03-15 19:35_

---

_Review comment by @zanieb on `crates/uv/src/commands/mod.rs`:79 on 2024-03-15 19:35_

`pip check` takes "0ms" with just a few packages — seems worth improving instead of displaying durations of zero.

---

_Review requested from @charliermarsh by @zanieb on 2024-03-15 20:29_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_check.rs`:56 on 2024-03-15 23:51_

Or "Audited", up to you.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_check.rs`:77 on 2024-03-15 23:52_

Diagnostics? I don't mind this language, it's just what we use internally.

---

_@charliermarsh approved on 2024-03-15 23:52_

Thx!

---

_@zanieb reviewed on 2024-03-15 23:58_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip_check.rs`:77 on 2024-03-15 23:58_

Diagnostic feels too agnostic whereas these are "problems", idk. We'd need to change the success message to say something other than "compatible" too.

---

_@zanieb reviewed on 2024-03-15 23:59_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip_check.rs`:56 on 2024-03-15 23:59_

Hm does it make sense to match our other use of "Audited" here? It might... but I feel like this context is different. I choose this message before I added a second saying all of the packages are compatible.

---

_Merged by @zanieb on 2024-03-18 17:49_

---

_Closed by @zanieb on 2024-03-18 17:49_

---

_Branch deleted on 2024-03-18 17:49_

---
