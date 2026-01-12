```yaml
number: 3101
title: "Read base requirements from `pyproject.toml` in `uv run`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - cli
assignees: []
merged: true
base: main
head: zb/uv-run-pyproject
created_at: 2024-04-17T18:02:19Z
updated_at: 2024-04-19T14:36:04Z
url: https://github.com/astral-sh/uv/pull/3101
synced_at: 2026-01-12T16:05:25Z
```

# Read base requirements from `pyproject.toml` in `uv run`

---

_@zanieb_

In addition to the requested requirements, we include requirements from a `pyproject.toml` file if it exists and install the current directory.

Closes https://github.com/astral-sh/uv/issues/3104

---

_Renamed from "Add `uv run` support for reading requirements from a `pyproject.toml` file" to "Read base requirements from `pyproject.toml` in `uv run`" by @zanieb on 2024-04-17 18:58_

---

_Label `internal` added by @zanieb on 2024-04-17 18:59_

---

_Label `cli` added by @zanieb on 2024-04-17 18:59_

---

_@zanieb reviewed on 2024-04-17 19:14_

---

_Review comment by @zanieb on `crates/uv/src/commands/run.rs`:114 on 2024-04-17 19:14_

cc @charliermarsh minor note for consideration in your design

---

_Marked ready for review by @zanieb on 2024-04-17 20:16_

---

_@zanieb reviewed on 2024-04-17 21:17_

---

_Review comment by @zanieb on `crates/uv/src/commands/run.rs`:53 on 2024-04-17 21:17_

This pattern closes https://github.com/astral-sh/uv/issues/3104

---

_@charliermarsh reviewed on 2024-04-18 00:34_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/run.rs`:114 on 2024-04-18 00:34_

I feel like this should still be looking up the ancestor hierarchy recursively?

---

_@charliermarsh approved on 2024-04-18 00:34_

---

_Review comment by @zanieb on `crates/uv/src/commands/run.rs`:114 on 2024-04-18 04:52_

Yes definitely, just not implementing it yet. I was hoping I could re-use some logic in the `uv-workspace` crate.

---

_@zanieb reviewed on 2024-04-18 04:52_

---

_@konstin approved on 2024-04-18 09:22_

---

_Merged by @zanieb on 2024-04-19 14:36_

---

_Closed by @zanieb on 2024-04-19 14:36_

---

_Branch deleted on 2024-04-19 14:36_

---
