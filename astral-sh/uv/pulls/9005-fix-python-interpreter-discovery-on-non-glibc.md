```yaml
number: 9005
title: Fix Python interpreter discovery on non-glibc hosts
type: pull_request
state: merged
author: dead10ck
labels:
  - bug
assignees: []
merged: true
base: main
head: android
created_at: 2024-11-11T02:16:50Z
updated_at: 2024-11-21T11:35:07Z
url: https://github.com/astral-sh/uv/pull/9005
synced_at: 2026-01-12T16:08:36Z
```

# Fix Python interpreter discovery on non-glibc hosts

---

_@dead10ck_

## Summary

On Termux, uv currently fails to find any interpreter because it can't find a glibc version, because there isn't one. But the Python interpreter is still functional nonetheless.

So, when glibc cannot be found, simply return 0 for the version numbers and mark the interpreter as being incompatible with manylinux

I really don't know if this is the right way to address this, but I can attest that manual testing shows uv appears to be fully functional, at least for pip and virtualenvs.

Fixes #7373

## Test Plan

I tried running the test suite, and after some tweaks, a good portion of the test suite passes as well. A significant number of tests fail, but this appears to be due to minor differences in output, like warnings about hard links not working (hard links are completely disallowed on Android), differences in the number of files removed, etc. The test suite seems to be very sensitive to minor variations in output.

---

_@samypr100 reviewed on 2024-11-11 04:10_

---

_Review comment by @samypr100 on `crates/uv-python/python/get_interpreter_info.py`:480 on 2024-11-11 04:10_

Given the declared wheel format in PEP-738, rather than allowing passing through the glibc version as (-1, -1)/(0, 0), I wonder if it's worthwhile considering using [sys.getandroidapilevel](https://docs.python.org/3/library/sys.html#sys.getandroidapilevel) here since it's compatible since 3.7+.

[PEP-738](https://peps.python.org/pep-0738/) also introduced [platform.android_ver](https://docs.python.org/3/library/platform.html#platform.android_ver), but sadly it's only available in 3.13.

e.g.
```python
        elif glibc_version != (-1, -1):
            operating_system = {
                "name": "manylinux",
                "major": glibc_version[0],
                "minor": glibc_version[1],
            }
        elif hasattr(sys, "getandroidapilevel"):
            operating_system = {
                "name": "android",
                "major": sys.getandroidapilevel(),
                "minor": 0,
            }
        else:
            print(json.dumps({"result": "error", "kind": "libc_not_found"}))
            sys.exit(0)
```
...
```python
    elif os_and_arch["os"]["name"] == "android":
        manylinux_compatible = False
```

---

_Review requested from @konstin by @zanieb on 2024-11-13 17:31_

---

_@konstin requested changes on 2024-11-14 12:40_

I agree with @samypr100, we need a way to specifically check for android; we had cases where the error branch caught bugs in the past

---

_Comment by @dead10ck on 2024-11-21 04:33_

Ok, I think I've done it. Manual testing seems to work.

---

_@konstin approved on 2024-11-21 09:01_

---

_Merged by @konstin on 2024-11-21 11:35_

---

_Closed by @konstin on 2024-11-21 11:35_

---

_Label `bug` added by @konstin on 2024-11-21 11:35_

---
