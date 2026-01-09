---
number: 4848
title: "\"No download found\" error should avoid debug representation"
type: issue
state: closed
author: charliermarsh
labels:
  - error messages
  - preview
assignees: []
created_at: 2024-07-06T19:11:15Z
updated_at: 2024-07-09T05:29:56Z
url: https://github.com/astral-sh/uv/issues/4848
synced_at: 2026-01-07T13:12:17-06:00
---

# "No download found" error should avoid debug representation

---

_Issue opened by @charliermarsh on 2024-07-06 19:11_

E.g., given `cargo run -- run -vv --preview --isolated --python 3.12.4 python -V`:

```
error: No download found for request: PythonDownloadRequest { version: Some(MajorMinorPatch(3, 12, 4)), implementation: Some(CPython), arch: Some(Arch(Aarch64(Aarch64))), os: Some(Os(Darwin)), libc: Some(None) }
```


---

_Label `error messages` added by @charliermarsh on 2024-07-06 19:11_

---

_Label `preview` added by @charliermarsh on 2024-07-06 19:11_

---

_Comment by @zanieb on 2024-07-06 19:14_

We should never throw this error here either, e.g. when `find_or_fetch` is called if no download can be found we should use the `MissingPython` case. We could add a special error case noting that we could not find the interpreter on the system or as an available download if we want to note that we looked for a download.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-08 14:31_

---

_Comment by @charliermarsh on 2024-07-08 14:50_

Okay got it, makes sense.

---

_Referenced in [astral-sh/uv#4913](../../astral-sh/uv/pulls/4913.md) on 2024-07-09 05:22_

---

_Closed by @charliermarsh on 2024-07-09 05:29_

---
