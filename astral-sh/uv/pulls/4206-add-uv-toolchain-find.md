```yaml
number: 4206
title: "Add `uv toolchain find`"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/toolchain-find
created_at: 2024-06-10T17:50:24Z
updated_at: 2024-06-14T17:03:18Z
url: https://github.com/astral-sh/uv/pull/4206
synced_at: 2026-01-12T16:06:05Z
```

# Add `uv toolchain find`

---

_@zanieb_

Adds a command to find a toolchain on the system. Right now, it displays the path to the first matching toolchain. We'll probably have more rich output in the future (after implementing `toolchain show`).

The eventual plan (separate from here) is to port all of the toolchain discovery tests to use this command. I'll add a few tests for this command here anyway.

---

_Label `preview` added by @zanieb on 2024-06-10 17:50_

---

_Marked ready for review by @zanieb on 2024-06-13 15:24_

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:2097 on 2024-06-13 23:56_

I somewhat expected this to just take `--python`. Why this schema?

---

_@charliermarsh reviewed on 2024-06-13 23:56_

---

_@charliermarsh approved on 2024-06-13 23:56_

---

_@zanieb reviewed on 2024-06-14 00:38_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:2097 on 2024-06-14 00:38_

Yeah uhh I was thinking it might be nice to present actual options just for usability and I'd provide a `--python`-like argument too. Maybe we should just take a positional argument that's treated like `--python`?

---

_Merged by @zanieb on 2024-06-14 17:03_

---

_Closed by @zanieb on 2024-06-14 17:03_

---

_Branch deleted on 2024-06-14 17:03_

---
