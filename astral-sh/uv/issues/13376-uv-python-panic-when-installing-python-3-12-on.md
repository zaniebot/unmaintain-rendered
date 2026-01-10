---
number: 13376
title: "uv-python: panic when installing Python 3.12 on Windows (called \\Option::unwrap()` on a `None` value in downloads.rs)`"
type: issue
state: closed
author: farukonfly
labels:
  - bug
assignees: []
created_at: 2025-05-10T09:36:39Z
updated_at: 2025-07-17T20:38:42Z
url: https://github.com/astral-sh/uv/issues/13376
synced_at: 2026-01-10T01:25:32Z
---

# uv-python: panic when installing Python 3.12 on Windows (called \Option::unwrap()` on a `None` value in downloads.rs)`

---

_Issue opened by @farukonfly on 2025-05-10 09:36_

### Question

no python in my pc

D:\>uv python install 3.12 --verbose
DEBUG uv 0.7.3 (3c413f74b 2025-05-07)
DEBUG Acquired lock for `C:\Users\farukon\AppData\Roaming\uv\python`
DEBUG No installation found for request `Python 3.12`
DEBUG Found download `cpython-3.12.10-windows-x86_64-none` for request `Python 3.12`
DEBUG Using request timeout of 30s

thread 'main2' panicked at C:\a\uv\uv\crates\uv-python\src\downloads.rs:646:14:
called `Option::unwrap()` on a `None` value
stack backtrace:
   0:     0x7ff6998f092c - <unknown>
   1:     0x7ff6994e121a - <unknown>
   2:     0x7ff6998efe87 - <unknown>
   3:     0x7ff6998f0784 - <unknown>
   4:     0x7ff6998efac5 - <unknown>
   5:     0x7ff699917132 - <unknown>
   6:     0x7ff6999170bf - <unknown>
   7:     0x7ff69991a7ae - <unknown>
   8:     0x7ff69b849601 - <unknown>
   9:     0x7ff69b84965d - <unknown>
  10:     0x7ff69b84988e - <unknown>
  11:     0x7ff699f99e99 - <unknown>
  12:     0x7ff699f952c2 - <unknown>
  13:     0x7ff6993d6a6d - <unknown>
  14:     0x7ff699e84602 - <unknown>
  15:     0x7ff699dde2e3 - <unknown>
  16:     0x7ff699dbffce - <unknown>
  17:     0x7ff69a50bf18 - <unknown>
  18:     0x7ff69a119262 - <unknown>
  19:     0x7ff699911b1d - <unknown>
  20:     0x7ffc1445e8d7 - BaseThreadInitThunk
  21:     0x7ffc149dbf6c - RtlUserThreadStart
DEBUG Released lock at `C:\Users\farukon\AppData\Roaming\uv\python\.lock`

thread 'main' panicked at C:\a\uv\uv\crates\uv\src\lib.rs:2106:10:
Tokio executor failed, was there a panic?: Any { .. }
stack backtrace:
   0:     0x7ff6998f092c - <unknown>
   1:     0x7ff6994e121a - <unknown>
   2:     0x7ff6998efe87 - <unknown>
   3:     0x7ff6998f0784 - <unknown>
   4:     0x7ff6998efac5 - <unknown>
   5:     0x7ff699917169 - <unknown>
   6:     0x7ff6999170bf - <unknown>
   7:     0x7ff69991a7ae - <unknown>
   8:     0x7ff69b849601 - <unknown>
   9:     0x7ff69b8499d0 - <unknown>
  10:     0x7ff69a63d2a2 - <unknown>
  11:     0x7ff69a507286 - <unknown>
  12:     0x7ff69a63c352 - <unknown>
  13:     0x7ff69b82357c - <unknown>
  14:     0x7ffc1445e8d7 - BaseThreadInitThunk
  15:     0x7ffc149dbf6c - RtlUserThreadStart

### Platform

Windows11 X86_64

### Version

uv 0.7.3 (3c413f74b 2025-05-07)

---

_Label `question` added by @farukonfly on 2025-05-10 09:36_

---

_Comment by @zanieb on 2025-05-10 13:35_

Thanks for the report!

https://github.com/astral-sh/uv/blob/878c2acdf30169af9f02bd23fe799f75916517e0/crates/uv-python/src/downloads.rs#L644-L646

cc @konstin 

---

_Label `question` removed by @zanieb on 2025-05-10 13:35_

---

_Label `bug` added by @zanieb on 2025-05-10 13:35_

---

_Comment by @charliermarsh on 2025-05-10 14:30_

Hmm, how is this empty there? Don't we create these URLs ourselves?

---

_Comment by @zanieb on 2025-05-10 14:36_

Have you set `UV_PYTHON_INSTALL_MIRROR` or `UV_PYTHON_DOWNLOADS_JSON_URL`?

---

_Comment by @zanieb on 2025-05-10 14:37_

(It's possible something has regressed here, as there have been changes in #10939, #12967, etc.)

---

_Referenced in [astral-sh/uv#13406](../../astral-sh/uv/pulls/13406.md) on 2025-05-12 11:23_

---

_Comment by @geofft on 2025-07-17 19:29_

It sounds like this should generate a proper error message in uv 0.7.4 and up that lists the invalid URL. If you see this again, it'd be interesting to get the error message.

---

_Closed by @konstin on 2025-07-17 20:38_

---

_Reopened by @konstin on 2025-07-17 20:38_

---

_Closed by @konstin on 2025-07-17 20:38_

---
