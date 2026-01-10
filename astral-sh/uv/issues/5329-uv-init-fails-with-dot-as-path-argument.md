---
number: 5329
title: "`uv init` fails with dot as path argument"
type: issue
state: closed
author: eth3lbert
labels:
  - bug
assignees: []
created_at: 2024-07-23T06:04:42Z
updated_at: 2024-07-23T12:20:15Z
url: https://github.com/astral-sh/uv/issues/5329
synced_at: 2026-01-10T01:23:47Z
---

# `uv init` fails with dot as path argument

---

_Issue opened by @eth3lbert on 2024-07-23 06:04_

```
/tmp                                                                                                                                                                                           13:41:13
:) mkdir example
/tmp                                                                                                                                                                                           13:41:19
:) cd example
/tmp/example                                                                                                                                                                                   13:41:20
:) uv init .    
warning: `uv init` is experimental and may change without warning
thread 'main' panicked at /Users/eth/workspace/astral-sh/uv/crates/uv/src/commands/project/init.rs:44:18:
Invalid package name
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Since this is a fairly common valid init argument, I believe it should not panic due to an `Invalid package name` error.

---

_Referenced in [astral-sh/uv#5330](../../astral-sh/uv/pulls/5330.md) on 2024-07-23 06:08_

---

_Label `bug` added by @konstin on 2024-07-23 08:36_

---

_Closed by @charliermarsh on 2024-07-23 12:20_

---

_Closed by @charliermarsh on 2024-07-23 12:20_

---
