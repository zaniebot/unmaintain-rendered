```yaml
number: 15424
title: "`find_uv_bin_py314` times out on Windows"
type: issue
state: open
author: zanieb
labels:
  - help wanted
  - ci-flake
assignees: []
created_at: 2025-08-21T18:24:27Z
updated_at: 2025-10-14T12:13:46Z
url: https://github.com/astral-sh/uv/issues/15424
synced_at: 2026-01-12T16:02:10Z
```

# `find_uv_bin_py314` times out on Windows

---

_@zanieb_

```
     TIMEOUT [ 120.100s] uv::it python_module::find_uv_bin_py314
  stdout ───

    running 1 test
    test python_module::find_uv_bin_py314 has been running for over 60 seconds

    (test timed out)
```

---

_Label `ci-flake` added by @zanieb on 2025-08-21 18:24_

---

_Label `help wanted` added by @zanieb on 2025-08-21 18:24_

---

_Comment by @Prhmma on 2025-10-11 11:27_

I couldn't reproduce the issue, 
With Python 3.14 uninstalled, the test fails fast (“Could not find Python 3.14 for test”), and with 3.14 installed, it succeeds. I didn’t see conditions that trigger the 60 s timeout in a clean environment.

---

_Comment by @konstin on 2025-10-14 12:13_

This problem is a CI flake, that means it only occurs sometimes, and we don't know yet why it is sometimes.

---
