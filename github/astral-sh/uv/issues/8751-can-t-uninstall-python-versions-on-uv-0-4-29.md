---
number: 8751
title: "Can't uninstall python versions on uv 0.4.29"
type: issue
state: closed
author: NovaliX-Dev
labels:
  - needs-mre
assignees: []
created_at: 2024-11-01T11:54:34Z
updated_at: 2024-11-01T13:55:26Z
url: https://github.com/astral-sh/uv/issues/8751
synced_at: 2026-01-07T13:12:18-06:00
---

# Can't uninstall python versions on uv 0.4.29

---

_Issue opened by @NovaliX-Dev on 2024-11-01 11:54_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I have Python 3.12.7 installed and manager by uv, and i want to uninstall it. Unfortunately, i don't manage to do so using the uv dedicated command.

Here is a list of all the commands i've tried : 
- `uv python uninstall 3.12`
- `uv python uninstall cp@3.12`
- `uv python uninstall cpython-3.12.7-linux-x86_64-gnu`
- `uv python uninstall cp@3.12.7`

All of them gives to me this output : 
```
Searching for Python versions matching: CPython 3.12.7
error: No such file or directory (os error 2)
```
With debug enabled (using `--verbose` and settings `RUST_LOG="trace"`):
```
DEBUG uv 0.4.29
TRACE Checking lock for `.local/share/uv/python` at `.local/share/uv/python/.lock`
DEBUG Acquired lock for `.local/share/uv/python`
Searching for Python versions matching: cpython-3.12.7-linux-x86_64-gnu
DEBUG Released lock at `/home/novalix/.local/share/uv/python/.lock`
error: No such file or directory (os error 2)
```

Here is all my installed python (using `uv python list --only-installed`):
```
cpython-3.13.0-linux-x86_64-gnu    /usr/bin/python3.13
cpython-3.13.0-linux-x86_64-gnu    /usr/bin/python3 -> python3.13
cpython-3.13.0-linux-x86_64-gnu    /usr/bin/python -> ./python3
cpython-3.13.0-linux-x86_64-gnu    /bin/python3.13
cpython-3.13.0-linux-x86_64-gnu    /bin/python3 -> python3.13
cpython-3.13.0-linux-x86_64-gnu    /bin/python -> ./python3
cpython-3.13.0-linux-x86_64-gnu    .local/share/uv/python/cpython-3.13.0-linux-x86_64-gnu/bin/python3.13
cpython-3.12.7-linux-x86_64-gnu    .local/share/uv/python/cpython-3.12.7-linux-x86_64-gnu/bin/python3.12
```

Installation information : 
- uv version : 0.4.29
- affected platforms : Fedora 41, Windows

I assume I'm doing something wrong, but i don't know what.
Thank you for your help in advance !

---

_Comment by @NovaliX-Dev on 2024-11-01 11:58_

The first two commands I've tried I would understand why they fail if i had many python installation with the same version and only the patch changing, but even though they fail the error they give is very confusing

---

_Comment by @ghost on 2024-11-01 12:14_

I also had same experience, i failed uninstall cpython@3.13 and 3.12 by uv uninstall command in my windows 11 24h2.

---

_Comment by @NovaliX-Dev on 2024-11-01 12:35_

I will update the title of the issue accordingly. 

---

_Renamed from "Can't uninstall python versions on Fedora 41" to "Can't uninstall python versions on uv 0.4.29" by @NovaliX-Dev on 2024-11-01 12:35_

---

_Comment by @charliermarsh on 2024-11-01 13:30_

It looks like a bug but the trace is really spartan, it's hard to know _what_ we're failing to find there.

---

_Label `needs-mre` added by @charliermarsh on 2024-11-01 13:31_

---

_Comment by @charliermarsh on 2024-11-01 13:31_

Are you able to reproduce this with a Dockerfile?

---

_Comment by @NovaliX-Dev on 2024-11-01 13:35_

I'm not really at ease with dockerfiles to be honest...

I will still try out with a dockerfile

---

_Comment by @zanieb on 2024-11-01 13:52_

Closed by https://github.com/astral-sh/uv/pull/8725 already — sorry about that.

---

_Closed by @charliermarsh on 2024-11-01 13:55_

---
