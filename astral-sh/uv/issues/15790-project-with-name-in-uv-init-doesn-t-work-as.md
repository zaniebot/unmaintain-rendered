---
number: 15790
title: "`--project` with name in `uv init` doesn't work as expect"
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2025-09-11T13:43:23Z
updated_at: 2025-11-30T15:14:50Z
url: https://github.com/astral-sh/uv/issues/15790
synced_at: 2026-01-10T01:25:59Z
---

# `--project` with name in `uv init` doesn't work as expect

---

_Issue opened by @konstin on 2025-09-11 13:43_

Currently, `uv init --project bar foo` ignores the `bar`.

---

_Assigned to @zanieb by @konstin on 2025-09-11 13:43_

---

_Label `bug` added by @konstin on 2025-09-11 13:43_

---

_Comment by @charliermarsh on 2025-09-11 13:46_

`--project` does take an argument. It means "run the command as if you were in the project at this path".

---

_Comment by @zanieb on 2025-09-11 14:19_

ref https://github.com/astral-sh/uv/blob/19ea0f4932940ceb9d9d436193cd4809b3172e68/crates/uv-cli/src/lib.rs#L344-L357

---

_Comment by @zanieb on 2025-09-11 14:22_

And here's the usage, which seems wrong https://github.com/astral-sh/uv/blob/289ed86e63f24017b4eb7bd0dfe6558fc0fdfaae/crates/uv/src/commands/project/init.rs#L99-L103

---

_Referenced in [astral-sh/uv#15791](../../astral-sh/uv/pulls/15791.md) on 2025-09-11 14:24_

---

_Renamed from "`--project` with name in `uv init` should error" to "`--project` with name in `uv init` doesn't work as expect" by @konstin on 2025-09-11 14:36_

---

_Comment by @konstin on 2025-09-11 14:37_

> `--project` does take an argument. It means "run the command as if you were in the project at this path".

oh I mixed up `--project` and `--package`. I updated the issue description.

---

_Referenced in [astral-sh/uv#16674](../../astral-sh/uv/pulls/16674.md) on 2025-11-10 19:30_

---

_Closed by @zanieb on 2025-11-30 15:14_

---
