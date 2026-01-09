---
number: 5620
title: "uv is trying to load lib from wrong dir `Library not loaded: /opt/homebrew/opt/xz/lib/liblzma.5.dylib`"
type: issue
state: closed
author: mdragoss
labels:
  - bug
assignees: []
created_at: 2024-07-30T17:47:25Z
updated_at: 2024-07-30T17:57:12Z
url: https://github.com/astral-sh/uv/issues/5620
synced_at: 2026-01-07T13:12:17-06:00
---

# uv is trying to load lib from wrong dir `Library not loaded: /opt/homebrew/opt/xz/lib/liblzma.5.dylib`

---

_Issue opened by @mdragoss on 2024-07-30 17:47_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

`uv --version` - 0.2.31

when I try to do something with uv get an error 
```
Referenced from: <297928D5-54B8-3A44-B1B4-41CE488775BD>
Reason: tried: '/opt/homebrew/opt/xz/lib/liblzma.5.dylib' (no such file), 
'/System/Volumes/Preboot/Cryptexes/OS/opt/homebrew/opt/xz/lib/liblzma.5.dylib' (no such file), '/opt/homebrew/opt/xz/lib/liblzma.5.dylib' (no such file)
```

not all libs are installed with homebrew

`find /usr/ | grep liblzma`

```
/usr//local/lib/pkgconfig/liblzma.pc
/usr//local/lib/liblzma.a
/usr//local/lib/liblzma.la
/usr//local/lib/liblzma.dylib
/usr//local/lib/liblzma.5.dylib
```



---

_Comment by @charliermarsh on 2024-07-30 17:55_

This is fixed on main, I'll release today.

---

_Closed by @charliermarsh on 2024-07-30 17:55_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-30 17:55_

---

_Label `bug` added by @charliermarsh on 2024-07-30 17:57_

---
