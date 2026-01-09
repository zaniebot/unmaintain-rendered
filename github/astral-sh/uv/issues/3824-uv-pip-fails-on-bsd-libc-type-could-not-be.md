---
number: 3824
title: "uv pip fails on BSD: Libc type could not be detected"
type: issue
state: closed
author: lcheylus
labels:
  - bug
assignees: []
created_at: 2024-05-24T15:02:38Z
updated_at: 2024-05-25T10:33:24Z
url: https://github.com/astral-sh/uv/issues/3824
synced_at: 2026-01-07T13:12:17-06:00
---

# uv pip fails on BSD: Libc type could not be detected

---

_Issue opened by @lcheylus on 2024-05-24 15:02_

Success to build of `uv` version 0.2.2 on OpenBSD current/amd64 with Rust version 1.78.0:
```bash
$ uv version
uv 0.2.2

$ rustc -vV
rustc 1.78.0 (15adfb9cd 2024-04-28) (built from a source tarball)
binary: rustc
commit-hash: 15adfb9cd5b029268e1470a41391d91859b0a1f8
commit-date: 2024-04-28
host: x86_64-unknown-openbsd
release: 1.78.0
LLVM version: 16.0.6
```

When I try to exec `uv pip list` command (or other `pip` commands), I have the following error:
```bash
$ uv pip list
error: Libc type could not be detected
```

After some analysis, this error seems to come from "Rewrite Python interpreter discovery" #3266. (release in version 0.2.0).

In `crates/uv-interpreter/src/platform.rs` file, function `Libc::from_env`, there is no case for BSD OD (`freebsd`, `openbsd`, `netbsd`), only for `linux` and `windows | macos` => `Error::LibcNotDetected` is returned for BSD OS.

I think this function sould return a new value `Libc::BSD` for BSD OS (specific libc version for this OS type, different from GNU libc).


---

_Referenced in [astral-sh/uv#3825](../../astral-sh/uv/pulls/3825.md) on 2024-05-24 16:47_

---

_Comment by @konstin on 2024-05-24 16:47_

Could you try with https://github.com/astral-sh/uv/pull/3825?

---

_Assigned to @konstin by @konstin on 2024-05-24 18:26_

---

_Comment by @lcheylus on 2024-05-25 00:30_

OK, PR #3825 fixes this issue:

- build of `uv` v0.2.3 + patch from #3825 on OpenBSD current/amd64 with Rust 1.78.0
- `uv pip <command>` works as attended
```bash
$ uv pip list
Package                       Version
----------------------------- ---------
alabaster                     0.7.16
astroid                       3.1.0
attrs                         23.2.0
autopep8                      2.0.4
(...)
```

Thanks @konstin for your fix.

---

_Comment by @zanieb on 2024-05-25 02:00_

Thank you for checking back in!

---

_Label `bug` added by @zanieb on 2024-05-25 02:00_

---

_Closed by @konstin on 2024-05-25 10:05_

---
