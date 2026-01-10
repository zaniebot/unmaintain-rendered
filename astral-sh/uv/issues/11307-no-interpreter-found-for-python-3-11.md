---
number: 11307
title: No interpreter found for Python 3.11
type: issue
state: closed
author: magdesign
labels:
  - bug
assignees: []
created_at: 2025-02-07T02:51:52Z
updated_at: 2025-03-12T02:08:55Z
url: https://github.com/astral-sh/uv/issues/11307
synced_at: 2026-01-10T01:25:03Z
---

# No interpreter found for Python 3.11

---

_Issue opened by @magdesign on 2025-02-07 02:51_

### Summary

Trying to build Servo on `musl` based **Alpine** Linux/PostmarketOS, aarch64.

` ./mach build --dev` fails with:

`error: No interpreter found for Python 3.11 in virtual environments, managed installations, or search path`

so i tried to manually 
`uv python install 3.11`

which fails with:
`error: uv does not yet provide musl Python distributions. See https://github.com/astral-sh/uv/issues/6890 to track support`

my `python --version` is `3.12.8`


Would be nice to mention in [build book](https://book.servo.org/hacking/building-servo.html) that musl based linux systems are not supported yet.

### Platform

postmarketaOS edge, aarch64

### Version

uv 0.5.29

### Python version

Python 3.12.8

---

_Label `bug` added by @magdesign on 2025-02-07 02:51_

---

_Comment by @konstin on 2025-02-07 08:15_

The musl python builds are tracked in #6890, please raise problems with servo builds in their bug tracker.

---

_Closed by @konstin on 2025-02-07 08:15_

---

_Comment by @zanieb on 2025-02-07 15:54_

Generally the workaround is just to install Python without uv (i.e., from the system package manager).

---

_Comment by @zanieb on 2025-03-12 02:08_

We support this now https://github.com/astral-sh/uv/pull/12121

---
