```yaml
number: 16628
title: Add expectation for externally managed marker in x86-64 macOS system test
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/extern-macos
created_at: 2025-11-07T04:11:55Z
updated_at: 2025-11-07T05:53:48Z
url: https://github.com/astral-sh/uv/pull/16628
synced_at: 2026-01-12T16:12:21Z
```

# Add expectation for externally managed marker in x86-64 macOS system test

---

_@zanieb_

This is failing with

```
Run python3 scripts/check_system_python.py --uv ./uv
INFO: Checking that `pylint` isn't installed.
WARNING: Package(s) not found: pylint
INFO: Installing the package `pylint`.
DEBUG uv 0.9.7 (2cd1400fb 2025-11-06)
DEBUG Acquired shared lock for `/Users/runner/.cache/uv`
DEBUG Searching for default Python interpreter in search path or managed installations
DEBUG Found `cpython-3.14.0-macos-x86_64-none` at `/usr/local/bin/python3` (first executable in the search path)
Using Python 3.14.0 environment at: /usr/local/opt/python@3.14/Frameworks/Python.framework/Versions/3.14
DEBUG Released lock at `/Users/runner/.cache/uv/.lock`
error: The interpreter at /usr/local/opt/python@3.14/Frameworks/Python.framework/Versions/3.14 is externally managed, and indicates the following:

  To install Python packages system-wide, try brew install
  xyz, where xyz is the package you are trying to
  install.

  If you wish to install a Python library that isn't in Homebrew,
  use a virtual environment:

  python3 -m venv path/to/venv
  source path/to/venv/bin/activate
  python3 -m pip install xyz

  If you wish to install a Python application that isn't in Homebrew,
  it may be easiest to use 'pipx install xyz', which will manage a
  virtual environment for you. You can install pipx with

  brew install pipx

  You may restore the old behavior of pip by passing
  the '--break-system-packages' flag to pip, or by adding
  'break-system-packages = true' to your pip.conf file. The latter
  will permanently disable this error.

  If you disable this error, we STRONGLY recommend that you additionally
  pass the '--user' flag to pip, or set 'user = true' in your pip.conf
  file. Failure to do this can result in a broken Homebrew installation.

  Read more about this behavior here: <https://peps.python.org/pep-0668/>
```

---

_Label `testing` added by @zanieb on 2025-11-07 04:11_

---

_Comment by @zanieb on 2025-11-07 04:13_

Weirdly this is a Homebrew Python but I don't think the GitHub runner should have that?

We're using setup-python

https://github.com/astral-sh/uv/blob/d1b3d36b1d3c098a98c861661949d1b418304220/.github/workflows/ci.yml#L2483-L2485

cc @woodruffw ?

---

_Comment by @zanieb on 2025-11-07 04:14_

Related https://github.com/astral-sh/uv/pull/16624

---

_Comment by @zanieb on 2025-11-07 04:50_

Hm, the logs are

```
un actions/setup-python@a26af69be951a213d495a4c3e4e4022e16d87065
  with:
    check-latest: false
    token: ***
    update-environment: true
    allow-prereleases: false
    freethreaded: false
  env:
    CARGO_INCREMENTAL: 0
    CARGO_NET_RETRY: 10
    CARGO_TERM_COLOR: always
    PYTHON_VERSION: 3.12
    RUSTUP_MAX_RETRIES: 10
    RUST_BACKTRACE: 1
Warning: Neither 'python-version' nor 'python-version-file' inputs were supplied. Attempting to find '.python-version' file.
Warning: .python-version doesn't exist.
Warning: The `python-version` input is not set.  The version of Python currently in `PATH` will be used.
```

---

_Comment by @zanieb on 2025-11-07 04:52_

Previously... (I picked an old commit) it looks like we were using a 3.13 framework interpreter?

```
Run python3 scripts/check_system_python.py --uv ./uv
INFO: Checking that `pylint` isn't installed.
WARNING: Package(s) not found: pylint
INFO: Installing the package `pylint`.
DEBUG uv 0.9.7 (17c9656df 2025-11-02)
DEBUG Acquired shared lock for `/Users/runner/.cache/uv`
DEBUG Searching for default Python interpreter in search path or managed installations
DEBUG Found `cpython-3.13.9-macos-x86_64-none` at `/usr/local/bin/python3` (first executable in the search path)
Using Python 3.13.9 environment at: /Library/Frameworks/Python.framework/Versions/3.13
DEBUG Acquired lock for `/Library/Frameworks/Python.framework/Versions/3.13`
DEBUG At least one requirement is not satisfied: pylint
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.13.9
DEBUG Solving with target Python version: >=3.13.9
DEBUG Adding direct dependency: pylint*
```

---

_Comment by @zanieb on 2025-11-07 05:53_

Let's unblock the failing CI first...

---

_Merged by @zanieb on 2025-11-07 05:53_

---

_Closed by @zanieb on 2025-11-07 05:53_

---

_Branch deleted on 2025-11-07 05:53_

---
