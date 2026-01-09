---
number: 16293
title: Failed to detect platform
type: issue
state: closed
author: NiuBlibing
labels:
  - bug
assignees: []
created_at: 2025-10-14T10:31:06Z
updated_at: 2025-10-15T07:45:12Z
url: https://github.com/astral-sh/uv/issues/16293
synced_at: 2026-01-07T13:12:19-06:00
---

# Failed to detect platform

---

_Issue opened by @NiuBlibing on 2025-10-14 10:31_

### Summary

I'm install nvitop by uv by local mirrors without windows packages sync(My local mirror only sync linux packages), it throws error:
```
  × No solution found when resolving dependencies:
  ╰─▶ Because there are no versions of windows-curses{sys_platform == 'win32'} and nvitop==1.5.3 depends on windows-curses{sys_platform == 'win32'}>=2.2.0, we can conclude
      that nvitop==1.5.3 cannot be used.
      And because only nvitop<=1.5.3 is available, we can conclude that nvitop>=1.5.3 cannot be used.
      And because your project depends on nvitop>=1.5.3 and your project requires test3[build], we can conclude that your project's requirements are unsatisfiable.
```
I try to add explicit sources and `required-environments`:
```

[[tool.uv.index]]
url = "https://mylocalmirror.net/simple"
default = true

[tool.uv.sources]
nvitop = { index = "tsinghua" }
windows-curses = { index = "tsinghua" }
colorama = { index = "tsinghua" }

[[tool.uv.index]]
name = "tsinghua"
url = "https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple"
explicit = true
```

```
[tool.uv]
# Require that the package is available for macOS ARM and x86 (Intel).
required-environments = [
    "sys_platform == 'linux'",,
]
```

### Platform

manjaro

### Version

uv 0.8.19 (fc7c2f8b5 2025-09-19)

### Python version

3.13.6

---

_Label `bug` added by @NiuBlibing on 2025-10-14 10:31_

---

_Comment by @konstin on 2025-10-14 11:41_

Does it work with `environments` instead of `required-environments`?

---

_Comment by @NiuBlibing on 2025-10-15 07:45_

It works, thanks a lot!

---

_Closed by @NiuBlibing on 2025-10-15 07:45_

---
