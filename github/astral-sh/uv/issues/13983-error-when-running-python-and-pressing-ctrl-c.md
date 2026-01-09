---
number: 13983
title: Error when running python and pressing ctrl+c using uv
type: issue
state: closed
author: obliqueresource
labels:
  - bug
assignees: []
created_at: 2025-06-12T06:58:53Z
updated_at: 2025-06-12T13:42:09Z
url: https://github.com/astral-sh/uv/issues/13983
synced_at: 2026-01-07T13:12:18-06:00
---

# Error when running python and pressing ctrl+c using uv

---

_Issue opened by @obliqueresource on 2025-06-12 06:58_

### Summary

When run python using uv and press `Ctrl+c` the KeyboardInterrupt kept on executing until press `Esc` key.

This is only exhibited when running python using uv. When python is run from normal distribution there is no error, so opening this ticket under uv.

Running uv in ghostty with fish shell.

```
> uv run python                                                     Thu Jun 12 14:51:35 2025
Python 3.12.11 (main, Jun  4 2025, 17:42:58) [Clang 20.1.4 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
KeyboardInterrupt
>>>
KeyboardInterrupt
>>>
KeyboardInterrupt
>>>
KeyboardInterrupt
>>>
KeyboardInterrupt
>>>
KeyboardInterrupt
>>>
KeyboardInterrupt
>>>
KeyboardInterrupt
>>>
```

With python 3.13.4 one time `Ctrl+c` sends it 2 times, something strange using python with uv.

### Platform

macOS 15.5

### Version

uv 0.7.12 (dc3fd4647 2025-06-06)

### Python version

Python 3.12.11

---

_Label `bug` added by @obliqueresource on 2025-06-12 06:58_

---

_Comment by @konstin on 2025-06-12 09:29_

That sounds like signal handling @oconnor663 @geofft 

---

_Comment by @zanieb on 2025-06-12 13:42_

Duplicate of #13919 — will be fixed in the next release.

---

_Closed by @zanieb on 2025-06-12 13:42_

---
