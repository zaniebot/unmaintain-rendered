---
number: 7964
title: uv python pin 3.13t gives a VersionParseError
type: issue
state: closed
author: olivierfriard
labels:
  - bug
assignees: []
created_at: 2024-10-07T10:30:54Z
updated_at: 2024-10-10T00:17:21Z
url: https://github.com/astral-sh/uv/issues/7964
synced_at: 2026-01-07T13:12:17-06:00
---

# uv python pin 3.13t gives a VersionParseError

---

_Issue opened by @olivierfriard on 2024-10-07 10:30_

Hi,

thank you for uv!

I am using uv (v0.4.18) on Linux.

The command:
`uv python pin 3.13t`
gives the following error message
```
thread 'main' panicked at crates/uv/src/commands/python/pin.rs:173:69:
called `Result::unwrap()` on an `Err` value: VersionParseError { kind: UnexpectedEnd { version: "3.13", remaining: "t" } }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

```

while 
`uv python pin 3.12t`
correctly reports
`error: Invalid version request: Python <3.13 does not support free-threading but 3.12t was requested.`


---

_Label `bug` added by @zanieb on 2024-10-07 12:52_

---

_Assigned to @zanieb by @zanieb on 2024-10-07 12:52_

---

_Comment by @zanieb on 2024-10-07 12:52_

Thanks I'll look into this.

---

_Referenced in [astral-sh/uv#8056](../../astral-sh/uv/pulls/8056.md) on 2024-10-09 19:56_

---

_Closed by @zanieb on 2024-10-10 00:17_

---
