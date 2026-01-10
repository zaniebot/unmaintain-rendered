---
number: 1397
title: "Add some sort of `cargo dev generate-all` command to run all build steps"
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2022-12-27T00:52:07Z
updated_at: 2022-12-27T15:07:19Z
url: https://github.com/astral-sh/ruff/issues/1397
synced_at: 2026-01-10T01:22:39Z
---

# Add some sort of `cargo dev generate-all` command to run all build steps

---

_Issue opened by @charliermarsh on 2022-12-27 00:52_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2022-12-27 00:52_

---

_Comment by @charliermarsh on 2022-12-27 13:38_

We should also just make this a build script.

---

_Comment by @charliermarsh on 2022-12-27 13:39_

We might want to disable formatting for `checks_gen.rs` to simplify that? So that the build step doesn't rely on `cargo +nightly fmt`? Not sure.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-27 13:54_

---

_Referenced in [astral-sh/ruff#1404](../../astral-sh/ruff/pulls/1404.md) on 2022-12-27 14:03_

---

_Closed by @charliermarsh on 2022-12-27 15:07_

---
