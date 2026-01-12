```yaml
number: 10815
title: "Creating virtual environment fails with \"`_sysconfigdata_` is missing a header comment\""
type: issue
state: closed
author: observingClouds
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-01-21T13:50:13Z
updated_at: 2025-01-21T14:25:02Z
url: https://github.com/astral-sh/uv/issues/10815
synced_at: 2026-01-12T16:00:22Z
```

# Creating virtual environment fails with "`_sysconfigdata_` is missing a header comment"

---

_@observingClouds_

Hi :wave:

Thanks for this great package! Really nice work.

**What I did**
I have an issue when creating a new virtual environment with a clean `uv` installation.

1. I removed a previous `uv` installation by following https://docs.astral.sh/uv/getting-started/installation/#uninstallation
2. I reinstalled `uv`
3. I requested a new virtual environment

```
$ curl -LsSf https://astral.sh/uv/install.sh | sh
downloading uv 0.5.21 x86_64-unknown-linux-gnu
no checksums to verify
installing to /home/has/.local/bin
  uv
  uvx
everything's installed!
$ uv venv --python=3.12
  × `_sysconfigdata_` is missing a header comment. # it is actually downloading the file
```

<details>
<summary>Full verbose traceback</summary>

```bash
$ uv venv --python=3.12 --verbose
DEBUG uv 0.5.21
DEBUG Using Python request `3.12` from explicit request
DEBUG Searching for Python 3.12 in managed installations or search path
DEBUG Searching for managed installations at `/home/has/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.12.8-linux-x86_64-gnu`
DEBUG Failed to inspect Python interpreter from managed installations at `/home/has/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu/bin/python3.12` 
DEBUG Skipping bad interpreter at /home/has/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu/bin/python3.12 from managed installations: Querying Python at `/home/has/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu/bin/python3.12` failed with exit status exit status: 1
--- stdout:

--- stderr:
Python path configuration:
  PYTHONHOME = (not set)
  PYTHONPATH = (not set)
  program name = '/home/has/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu/bin/python3.12'
  isolated = 1
  environment = 0
  user site = 0
  safe_path = 1
  import site = 1
  is in build tree = 0
  stdlib dir = '/home/has/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu/lib/python3.12'
  sys._base_executable = '/home/has/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu/bin/python3.12'
  sys.base_prefix = '/home/has/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu'
  sys.base_exec_prefix = '/home/has/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu'
  sys.platlibdir = 'lib'
  sys.executable = '/home/has/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu/bin/python3.12'
  sys.prefix = '/home/has/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu'
  sys.exec_prefix = '/home/has/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu'
  sys.path = [
    '/home/has/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu/lib/python312.zip',
    '/home/has/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu/lib/python3.12',
    '/home/has/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu/lib/python3.12/lib-dynload',
  ]
Fatal Python error: init_fs_encoding: failed to get the Python codec of the filesystem encoding
Python runtime state: core initialized
LookupError: no codec search functions registered: can't find encoding

Current thread 0x00007f2bdfc5a740 (most recent call first):
  <no Python frame>
---
DEBUG Skipping incompatible managed installation `cpython-3.11.11-linux-x86_64-gnu`
DEBUG Found `cpython-3.10.14-linux-x86_64-gnu` at `/dmidata/users/has/git/.venv/bin/python3` (search path)
DEBUG Ignoring Python interpreter at `/dmidata/users/has/git/.venv/bin/python3`: system interpreter required
DEBUG Found `cpython-3.10.14-linux-x86_64-gnu` at `/dmidata/users/has/git/.venv/bin/python` (search path)
DEBUG Ignoring Python interpreter at `/dmidata/users/has/git/.venv/bin/python`: system interpreter required
DEBUG Found `cpython-3.10.14-linux-x86_64-gnu` at `/tmp/squashed-has/store/miniforge3/bin/python3` (search path)
DEBUG Skipping interpreter at `/tmp/squashed-has/store/miniforge3/bin/python3` from search path: does not satisfy request `3.12`
DEBUG Found `cpython-3.10.14-linux-x86_64-gnu` at `/tmp/squashed-has/store/miniforge3/bin/python` (search path)
DEBUG Skipping interpreter at `/tmp/squashed-has/store/miniforge3/bin/python` from search path: does not satisfy request `3.12`
DEBUG Found `cpython-3.8.10-linux-x86_64-gnu` at `/usr/bin/python3` (search path)
DEBUG Skipping interpreter at `/usr/bin/python3` from search path: does not satisfy request `3.12`
DEBUG Failed to inspect Python interpreter from search path at `/usr/bin/python` 
DEBUG Skipping bad interpreter at /usr/bin/python from search path: Can't use Python at `/usr/bin/python`
DEBUG Found `cpython-3.8.10-linux-x86_64-gnu` at `/bin/python3` (search path)
DEBUG Skipping interpreter at `/bin/python3` from search path: does not satisfy request `3.12`
DEBUG Failed to inspect Python interpreter from search path at `/bin/python` 
DEBUG Skipping bad interpreter at /bin/python from search path: Can't use Python at `/bin/python`
DEBUG Requested Python not found, checking for available download...
DEBUG Acquired lock for `/home/has/.local/share/uv/python`
DEBUG Using request timeout of 30s
INFO Fetching requested Python...
DEBUG Released lock at `/home/has/.local/share/uv/python/.lock`
  × `_sysconfigdata_` is missing a header comment
```

</details>

For python 3.11 the initial error is the same after the download from the web, afterwards it is

```bash
$ uv venv --python=3.11 --verbose
DEBUG uv 0.5.21
DEBUG Using Python request `3.11` from explicit request
DEBUG Searching for Python 3.11 in managed installations or search path
DEBUG Searching for managed installations at `/home/has/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.12.8-linux-x86_64-gnu`
DEBUG Found managed installation `cpython-3.11.11-linux-x86_64-gnu`
DEBUG Failed to inspect Python interpreter from managed installations at `/home/has/.local/share/uv/python/cpython-3.11.11-linux-x86_64-gnu/bin/python3.11` 
  × Failed to inspect Python interpreter from managed installations at `/home/has/.local/share/uv/python/cpython-3.11.11-linux-x86_64-gnu/bin/python3.11`
  ├─▶ Failed to query Python interpreter at `/home/has/.local/share/uv/python/cpython-3.11.11-linux-x86_64-gnu/bin/python3.11`
  ╰─▶ Exec format error (os error 8)
```

**What I expected**
I expected the installation to work without issues. I have done this before but something has changed now. Could there be some broken files somewhere?

**System info**
Linux **** 5.4.0-200-generic #220-Ubuntu SMP Fri Sep 27 13:19:16 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux

---

_Comment by @charliermarsh on 2025-01-21 13:53_

Can you look in `/home/has/.local/share/uv/python/cpython-3.12..../` for a file that starts with `_sysconfigdata_`?

---

_Label `needs-mre` added by @charliermarsh on 2025-01-21 13:53_

---

_Comment by @observingClouds on 2025-01-21 14:02_

@charliermarsh, thanks for your fast response 🚀 .
I have `~/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu/lib/python3.12/_sysconfigdata__linux_x86_64-linux-gnu.py` which is empty.

---

_Comment by @charliermarsh on 2025-01-21 14:03_

Empty?! Ok, does that reproduce if you `rm -rf /home/has/.local/share/uv` and then re-run `uv venv --python=3.12`?

---

_Comment by @observingClouds on 2025-01-21 14:06_

Yes, following those two steps I end up with the same error.

---

_Comment by @observingClouds on 2025-01-21 14:07_

And the file `cat ~/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu/lib/python3.12/_sysconfigdata__linux_x86_64-linux-gnu.py` remains empty

---

_Comment by @charliermarsh on 2025-01-21 14:12_

There's something very strange going on but it's hard to tell what it is from here. Are you able to reproduce this in a Dockerfile? I assume not, and that this is on your own machine?

---

_Label `bug` added by @charliermarsh on 2025-01-21 14:14_

---

_Comment by @observingClouds on 2025-01-21 14:15_

It is on a linux cluster where I do not have root privileges, unfortunately.

---

_Comment by @observingClouds on 2025-01-21 14:19_

I cleared out my environment a bit and now get a different message that is more helpful:
```
uv venv --python=3.11
  × failed to sync file `/home/has/.local/share/uv/python/cpython-3.11.11-linux-x86_64-gnu/lib/python3.11/_sysconfigdata__linux_x86_64-linux-gnu.py`: Disk quota exceeded (os error 122)
```

I close the issue if this was the problem after cleaning up.

---

_Comment by @charliermarsh on 2025-01-21 14:21_

Ohh, that makes more sense, I guess.

---

_Closed by @charliermarsh on 2025-01-21 14:25_

---
