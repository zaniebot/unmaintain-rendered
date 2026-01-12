```yaml
number: 14714
title: Python version removal from the Windows registry can race
type: issue
state: closed
author: zanieb
labels:
  - bug
assignees: []
created_at: 2025-07-18T11:50:32Z
updated_at: 2025-07-18T12:47:57Z
url: https://github.com/astral-sh/uv/issues/14714
synced_at: 2026-01-12T16:01:55Z
```

# Python version removal from the Windows registry can race

---

_@zanieb_

### Summary

e.g., this flake in CI

```
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    Snapshot: python_install_default-3
    Source: V:\uv:1215
    ───────────────────────────────────────────────────────────────────────────────
    Expression: snapshot
    ───────────────────────────────────────────────────────────────────────────────
    -old snapshot
    +new results
    ────────────┬──────────────────────────────────────────────────────────────────
        2     2 │ ----- stdout -----
        3     3 │ 
        4     4 │ ----- stderr -----
        5     5 │ Searching for Python installations
              6 │+warning: Failed to remove orphan registry key HKCU:/Software/Python/Astral/CPython3.9.18: The system cannot find the file specified. (0x80070002)
        6     7 │ Uninstalled Python 3.13.5 in [TIME]
        7     8 │  - cpython-3.13.5-[PLATFORM] (python, python3, python3.13)
```

### Platform

Windows

### Version

main

### Python version

n/a

---

_Label `bug` added by @zanieb on 2025-07-18 11:50_

---

_Comment by @zanieb on 2025-07-18 11:51_

This may be unique to the test suite, we could turn off the registry in some of those tests? but we probably want this to be silent

---

_Comment by @konstin on 2025-07-18 12:07_

The warning looks correct in the sense that it sees a key that is indeed dangling, and that we can't do it better as there's no transaction-like concept where we could make the change to the filesystem and the one to the register at the same time. We should just turn the registry off for the tests, we don't want to change a dev machine's registry anyway.

---

_Comment by @zanieb on 2025-07-18 12:12_

I think if we fail to remove it because it was already removed, we shouldn't report a warning? `The system cannot find the file specified. (0x80070002)` being the key part of the error.

---

_Comment by @zanieb on 2025-07-18 12:12_

I don't have any qualms with the warning in general, but I think we could handle this case and just make it concurrency safe

---

_Comment by @zanieb on 2025-07-18 12:16_

We actually handle this in other cases, I'll just put up a PR

---

_Comment by @konstin on 2025-07-18 12:18_

I'm assuming that the case we're hitting is a TOCTOU race condition:

* A removes the files for CPython3.9.18
* B sees the key CPython3.9.18
* B sees that CPython3.9.18 has no files
* A removes the key for CPython3.9.18
* B try to removes the key for CPython3.9.18, sees it's already gone

So we can handle this both by making this more resilient on removal, and we can fix this by removing the registry key first, and then uninstall.

---

_Comment by @konstin on 2025-07-18 12:19_

I have the change ready

---

_Closed by @konstin on 2025-07-18 12:47_

---
