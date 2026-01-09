---
number: 12565
title: Misleading error message when build backend returns a non-compliant wheel filename
type: issue
state: closed
author: jaraco
labels:
  - error messages
assignees: []
created_at: 2025-03-30T21:37:17Z
updated_at: 2025-04-07T14:28:48Z
url: https://github.com/astral-sh/uv/issues/12565
synced_at: 2026-01-07T13:12:18-06:00
---

# Misleading error message when build backend returns a non-compliant wheel filename

---

_Issue opened by @jaraco on 2025-03-30 21:37_

### Summary

As reported in coherent-oss/coherent.build#34, attempting to build the coherent.build project fails with two errors:

```
 coherent.build main 🐚 uv pip install .
Resolved 213 packages in 3.77s
  × Failed to build `coherent-build @ file:///Users/jaraco/code/coherent-oss/coherent.build`
  ├─▶ Built wheel has an invalid filename
  ╰─▶ The wheel filename "/Users/jaraco/Library/Caches/uv/builds-v0/.tmpS2lnVm/coherent_build-0.28.0-py3-none-any.whl" has an invalid package name
```

How's it invalid? Pip and PyPI allow it.

### Platform

macOS 15.3

### Version

uv 0.6.10 (Homebrew 2025-03-26)

### Python version

3.13

---

_Label `bug` added by @jaraco on 2025-03-30 21:37_

---

_Comment by @konstin on 2025-03-30 23:33_

The build backend must return the basename of the `.whl` file it creates, but the build backend returns the full path (https://github.com/coherent-oss/coherent.build/blob/d75a013cb4f1d38024ac09e551f98d8853032aa5/backend.py#L158). uv splits the returned string at the dashes and then fails to parse `/Users/jaraco/Library/Caches/uv/builds-v0/.tmpS2lnVm/coherent_build` as wheel filename, returning the misleading error message.

---

_Label `bug` removed by @konstin on 2025-03-30 23:33_

---

_Label `question` added by @konstin on 2025-03-30 23:33_

---

_Label `question` removed by @konstin on 2025-03-30 23:34_

---

_Label `error messages` added by @konstin on 2025-03-30 23:34_

---

_Renamed from "Build wheel has an invalid filename" to "Misleading error message when build backend returns a non-compliant wheel filename" by @zanieb on 2025-04-01 21:31_

---

_Comment by @jaraco on 2025-04-05 18:54_

Thank you! I see now [the spec](https://peps.python.org/pep-0517/#build-wheel) makes that clear. I'd have thought to look there myself except things were working in other tools (didn't affect `pip` or `build`).

---

_Closed by @jaraco on 2025-04-05 18:54_

---

_Comment by @notatallshaw on 2025-04-07 14:28_

I'll take a look at some point why pip doesn't also fail. 

---
