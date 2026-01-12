```yaml
number: 15035
title: "`uv sync --python-platform` does not update environment when switching to a lower GLIBC-bound platform"
type: issue
state: closed
author: ido123net
labels:
  - bug
assignees: []
created_at: 2025-08-02T23:57:11Z
updated_at: 2025-08-29T05:44:08Z
url: https://github.com/astral-sh/uv/issues/15035
synced_at: 2026-01-12T16:02:02Z
```

# `uv sync --python-platform` does not update environment when switching to a lower GLIBC-bound platform

---

_@ido123net_

### Summary

When using uv sync with the new --python-platform flag to target a lower GLIBC version (e.g. manylinux_2_17), the environment is not updated if it was previously created with a higher GLIBC-bound platform (e.g. manylinux_2_28). This results in runtime errors due to incompatible native binaries remaining in the environment.

Steps to Reproduce (on system with GLIBC < 2.28  e.g., Ubuntu 18.04):
```bash
# Setup a demo project
$ uv init demo && cd demo

# Add cryptography (will resolve and install for system default or manylinux_2_28)
$ uv add cryptography

# Confirm it fails due to GLIBC_2.28 requirement
$ echo "from cryptography.hazmat.bindings._rust import openssl" > main.py
$ uv run main.py
# Traceback:
# ImportError: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.28' not found ...

# Now sync with a lower GLIBC platform
$ uv sync --python-platform x86_64-manylinux_2_17

# Try again still fails with same error
$ uv run main.py
# ImportError: ... version `GLIBC_2.28' not found ...
```

### Expected Behavior:

Running `uv sync --python-platform x86_64-manylinux_2_17` should re-resolve and reinstall packages so that native wheels compatible with manylinux_2_17 (and GLIBC < 2.28) are used. Instead, it leaves the environment unchanged.

### Actual Behavior:

The environment remains as-is, and uv does not detect the need to downgrade the platform-specific binaries, causing continued import/runtime failures.

Related Issues:
- #14273
- #14320
- #11120 (opened by me as well)

I understand this is an edge case, but supporting this behavior would be very helpful, especially in environments where the same project directory is accessed from multiple platforms via NFS or similar setups. Ensuring uv sync fully respects the --python-platform flag, even when downgrading, would make uv far more reliable and predictable in such cross-platform workflows.

### Platform

Linux

### Version

uv 0.8.4

### Python version

_No response_

---

_Label `bug` added by @ido123net on 2025-08-02 23:57_

---

_Comment by @ghost on 2025-08-04 13:00_

The same happens if you switch platform completely (for example from windows to linux). uv does not reinstall the required wheels for the new platform. 

Example output from a project with SQLAlchemy dependency:

```
> uv sync --no-editable --no-dev --python 3.10 --python-platform linux
...
 + greenlet==3.2.3
 + pyasn1==0.6.1
 + rsa==4.9.1
 + sqlalchemy==2.0.42
 + typing-extensions==4.14.1

> uv sync --no-editable --no-dev --python 3.10 --python-platform windows --dry-run
...
Would make no changes
> uv sync --no-editable --no-dev --python 3.10 --python-platform windows
...
Audited 7 packages in 0.34ms
```

Actual behavior: sqlalchemy with Windows extensions modules (pyd) is still installed.
Expected behavior: uv reinstalls sqlalchemy with Linux extension modules.


---

_Comment by @charliermarsh on 2025-08-04 13:23_

I'm a little unsure if we should support this. We certainly could (as in, it's not technically infeasible).

---

_Comment by @ido123net on 2025-08-04 13:42_

I will be happy to give it a shot.

If it is ok from your side. @charliermarsh 

---

_Comment by @charliermarsh on 2025-08-04 18:00_

Please feel free if you'd like to try. I think you need to read the `WHEEL` file from the installed packages and compare the wheel tag to the current platform.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-24 12:35_

---

_Comment by @charliermarsh on 2025-08-24 21:05_

I have this working at https://github.com/astral-sh/uv/pull/15484.

---

_Closed by @charliermarsh on 2025-08-25 13:20_

---

_Comment by @ido123net on 2025-08-29 05:44_

Thank you so much!!  

I had a bit of time to work on it, but I only managed to set up a working development environment for testing.  

I definitely couldn’t have reached your implementation on my own.  

This will help my workflow a ton 😄

---
