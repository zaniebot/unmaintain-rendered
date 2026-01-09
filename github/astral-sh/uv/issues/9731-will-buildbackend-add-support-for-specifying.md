---
number: 9731
title: Will buildbackend add support for specifying platform_tag?
type: issue
state: open
author: xqm32
labels:
  - question
assignees: []
created_at: 2024-12-09T07:26:53Z
updated_at: 2025-01-07T20:22:42Z
url: https://github.com/astral-sh/uv/issues/9731
synced_at: 2026-01-07T13:12:18-06:00
---

# Will buildbackend add support for specifying platform_tag?

---

_Issue opened by @xqm32 on 2024-12-09 07:26_

In a Python project that depends on ccfi, some dynamic libraries are included in the Wheel, but the resulting package is a generic wheel. If we could specify a platform_tag, it would allow different dynamic libraries to be used for different platforms when packaging the wheel.

---

_Comment by @zanieb on 2025-01-07 19:26_

cc @konstin 

---

_Label `question` added by @konstin on 2025-01-07 20:22_

---

_Comment by @konstin on 2025-01-07 20:22_

This will depend on having build steps or plugins, with which specific tags will be possible. Right now it's too early, eventually there will be options around this.

---
