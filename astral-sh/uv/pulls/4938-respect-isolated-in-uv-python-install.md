```yaml
number: 4938
title: "Respect `--isolated` in `uv python install`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: zb/install-isolated
created_at: 2024-07-09T19:37:42Z
updated_at: 2024-07-10T15:36:26Z
url: https://github.com/astral-sh/uv/pull/4938
synced_at: 2026-01-10T13:42:52Z
```

# Respect `--isolated` in `uv python install`

---

_Pull request opened by @zanieb on 2024-07-09 19:37_

We ignore Python version files when `--isolated` is used, logging that we skipped them if they exist.

---

_Label `cli` added by @zanieb on 2024-07-09 19:37_

---

_Label `preview` added by @zanieb on 2024-07-09 19:44_

---

_@charliermarsh reviewed on 2024-07-09 20:56_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:52 on 2024-07-09 20:56_

Some implicit coupling here between `requests_from_version_file` and the paths you're checking... Maybe just make shared `PYTHON_VERSION` and `PYTHON_VERSIONS` constants and reference them in both places, so the connection is clear in case we change them later?

---

_@charliermarsh approved on 2024-07-09 20:56_

---

_@zanieb reviewed on 2024-07-09 21:10_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:52 on 2024-07-09 21:10_

I could do that, will need in `pin` too.

---

_Merged by @zanieb on 2024-07-10 15:36_

---

_Closed by @zanieb on 2024-07-10 15:36_

---

_Branch deleted on 2024-07-10 15:36_

---
