---
number: 7685
title: An invalid netrc file does not produce a warning
type: issue
state: closed
author: mathialo
labels:
  - help wanted
  - error messages
  - tracing
assignees: []
created_at: 2024-09-25T13:16:12Z
updated_at: 2024-10-20T16:27:45Z
url: https://github.com/astral-sh/uv/issues/7685
synced_at: 2026-01-10T01:24:18Z
---

# An invalid netrc file does not produce a warning

---

_Issue opened by @mathialo on 2024-09-25 13:16_

I recently migrated one of our projects from poetry to uv, and one thing that made me stumble was a syntax error in my netrc file. Uv would just silently ignore the error and move on as if the netrc file did not exist, which made it a bit hard to figure out what was going on.

I think a better approach would be if the [`Netrc::new()`](https://github.com/astral-sh/uv/blob/84e5f6e871f344aa8b414c52bf95b22a992b382d/crates/uv-auth/src/middleware.rs#L33) call returns a _parsing error_, to log a warning about it before ignoring it, to let users know why the netrc file is being ignored.

---

_Label `error messages` added by @zanieb on 2024-09-25 14:40_

---

_Label `tracing` added by @zanieb on 2024-09-25 14:40_

---

_Comment by @zanieb on 2024-09-25 14:41_

Seems reasonable, but we'd want to be careful to only display this once and only if we needed credentials.

---

_Label `help wanted` added by @zanieb on 2024-09-25 14:41_

---

_Referenced in [astral-sh/uv#8364](../../astral-sh/uv/pulls/8364.md) on 2024-10-19 13:47_

---

_Closed by @charliermarsh on 2024-10-20 16:27_

---

_Closed by @charliermarsh on 2024-10-20 16:27_

---
