```yaml
number: 2729
title: "Use python3.8 for 'python on macos aarch64'"
type: pull_request
state: closed
author: charliermarsh
labels:
  - testing
assignees: []
base: main
head: charlie/macos
created_at: 2024-03-30T02:33:36Z
updated_at: 2024-03-30T14:55:18Z
url: https://github.com/astral-sh/uv/pull/2729
synced_at: 2026-01-12T16:05:11Z
```

# Use python3.8 for 'python on macos aarch64'

---

_@charliermarsh_

_No description provided._

---

_Label `testing` added by @charliermarsh on 2024-03-30 02:33_

---

_Marked ready for review by @zanieb on 2024-03-30 14:13_

---

_Comment by @zanieb on 2024-03-30 14:13_

Prioritizing because the cache check fails now with the same thing https://github.com/astral-sh/uv/actions/runs/8491249792/job/23263080140

They must have changed the macOS 14 runners.

---

_@zanieb reviewed on 2024-03-30 14:14_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:274 on 2024-03-30 14:14_

No real reason for this test to use brew's Python. We may want to change that.

---

_@zanieb approved on 2024-03-30 14:18_

---

_@charliermarsh reviewed on 2024-03-30 14:23_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yml`:274 on 2024-03-30 14:23_

I think it’s intentional to have a test for Brew Pythons though.

---

_@zanieb reviewed on 2024-03-30 14:25_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:274 on 2024-03-30 14:25_

Yeah but we don't need it in the cache compatibility test, just the system check.

---

_Comment by @zanieb on 2024-03-30 14:35_

Reproduced this locally

```
❯ /opt/homebrew/bin/python3.9 scripts/check_system_python.py
INFO: Checking that `pylint` isn't installed.
WARNING: Package(s) not found: pylint
INFO: Installing the package `pylint`.
Resolved 7 packages in 5ms
Installed 1 package in 10ms
 + pylint==3.1.0
INFO: Checking that `pylint` is installed.
WARNING: Package(s) not found: pylint
Traceback (most recent call last):
  File "/Users/mz/workspace/uv/scripts/check_system_python.py", line 82, in <module>
    raise Exception("The package `pylint` isn't installed (but should be).")
Exception: The package `pylint` isn't installed (but should be).

❯ /opt/homebrew/bin/python3.9 -m pip list
Package    Version
---------- -------
pip        23.3.1
setuptools 68.2.2
wheel      0.41.3

❯ ls /opt/homebrew/lib/python3.9/site-packages
_distutils_hack			pip-23.3.1.dist-info		setuptools-68.2.2.dist-info
distutils-precedence.pth	pkg_resources			wheel
pip				setuptools			wheel-0.41.3.dist-info
```

---

_Comment by @zanieb on 2024-03-30 14:39_

`/Library/Frameworks/Python.framework/Versions/3.12/bin/python3` is being used on my machine instead of the HomeBrew Python.

---

_Comment by @zanieb on 2024-03-30 14:55_

Superseded by #2735 and #2736 

---

_Closed by @zanieb on 2024-03-30 14:55_

---
