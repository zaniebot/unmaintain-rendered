```yaml
number: 14546
title: "Tear miette out of the `uv venv` command"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
  - breaking
assignees: []
merged: true
base: release/080
head: zb/venv
created_at: 2025-07-10T17:26:32Z
updated_at: 2025-07-24T20:47:00Z
url: https://github.com/astral-sh/uv/pull/14546
synced_at: 2026-01-10T06:53:02Z
```

# Tear miette out of the `uv venv` command

---

_Pull request opened by @zanieb on 2025-07-10 17:26_

This has some changes to the user-facing output, but makes it more consistent with the rest of uv.

---

_Label `cli` added by @zanieb on 2025-07-10 17:26_

---

_Review comment by @zanieb on `crates/uv/tests/it/venv.rs`:659 on 2025-07-10 17:29_

I think these error code changes are fine?

---

_@zanieb reviewed on 2025-07-10 17:29_

---

_Comment by @zanieb on 2025-07-10 18:49_

We could choose to consider this breaking.

---

_@charliermarsh approved on 2025-07-10 23:07_

---

_Comment by @charliermarsh on 2025-07-10 23:08_

We should probably try to remove this dep entirely (not here).

---

_Label `breaking` added by @zanieb on 2025-07-11 12:35_

---

_Merged by @zanieb on 2025-07-11 12:47_

---

_Closed by @zanieb on 2025-07-11 12:47_

---

_Branch deleted on 2025-07-11 12:47_

---

_@gaborbernat reviewed on 2025-07-24 20:37_

---

_Review comment by @gaborbernat on `crates/uv/tests/it/venv.rs`:659 on 2025-07-24 20:37_

FWIW, this did break tox-uv, https://github.com/tox-dev/tox-uv/pull/223.

---

_@zanieb reviewed on 2025-07-24 20:45_

---

_Review comment by @zanieb on `crates/uv/tests/it/venv.rs`:659 on 2025-07-24 20:45_

Ah, sorry about that.

---

_Review comment by @gaborbernat on `crates/uv/tests/it/venv.rs`:659 on 2025-07-24 20:46_

No worries, we released a fix, though internally at my company we have a 7 day delay ingest... that did cause some issues, as we only discovered the issue a few days ago, so pulling in the fix too will take a few days 🤦 

---

_@gaborbernat reviewed on 2025-07-24 20:47_

---
