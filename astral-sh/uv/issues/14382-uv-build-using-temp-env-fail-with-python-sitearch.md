---
number: 14382
title: uv build using temp env fail with Python_SITEARCH in CMake
type: issue
state: open
author: habaohaba
labels:
  - needs-mre
assignees: []
created_at: 2025-07-01T03:03:16Z
updated_at: 2025-07-01T13:58:52Z
url: https://github.com/astral-sh/uv/issues/14382
synced_at: 2026-01-10T01:25:44Z
---

# uv build using temp env fail with Python_SITEARCH in CMake

---

_Issue opened by @habaohaba on 2025-07-01 03:03_

### Summary

when using uv pip install, it seems like uv create a tmp env and build the package. But the tmp env can out find the pacakge required using Python_SITEARCH env var in CMake.
Is it a bad habbit to use CMake in this way?
What is the recommended way to write cmake for uv build.

### Platform

Linux 5.4.119-19-0009.11 x86_64 GNU/Linux

### Version

uv 0.7.5

### Python version

Python 3.10.12

---

_Label `bug` added by @habaohaba on 2025-07-01 03:03_

---

_Renamed from "uv build using temp env which doesn't have enough package for building" to "uv build using temp env fail with Python_SITEARCH in CMake" by @habaohaba on 2025-07-01 03:04_

---

_Label `bug` removed by @konstin on 2025-07-01 13:58_

---

_Label `needs-mre` added by @konstin on 2025-07-01 13:58_

---

_Comment by @konstin on 2025-07-01 13:58_

Can you provide a minimal reproducible example for your problem (https://github.com/astral-sh/uv/issues/9452)?

---

_Referenced in [astral-sh/uv#15415](../../astral-sh/uv/issues/15415.md) on 2025-08-21 13:54_

---
