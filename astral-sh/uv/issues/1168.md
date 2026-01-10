```yaml
number: 1168
title: "Properly handle `python.exe` xor `py` being present on windows"
type: issue
state: closed
author: konstin
labels:
  - error messages
  - windows
assignees: []
created_at: 2024-01-29T12:07:52Z
updated_at: 2024-02-22T07:47:35Z
url: https://github.com/astral-sh/uv/issues/1168
synced_at: 2026-01-10T05:40:31Z
```

# Properly handle `python.exe` xor `py` being present on windows

---

_Issue opened by @konstin on 2024-01-29 12:07_

We currently assume `py`, the python launcher on windows, is always present. We should test without it and allow only `python.exe` and show good error messages when both are missing.

---

_Label `error messages` added by @konstin on 2024-01-29 12:07_

---

_Label `windows` added by @konstin on 2024-01-29 12:07_

---

_Renamed from "Properly handle `python.ex` xor `py` being present on windows" to "Properly handle `python.exe` xor `py` being present on windows" by @konstin on 2024-01-29 12:23_

---

_Comment by @mjclarke94 on 2024-02-15 22:05_

There's also [python-launcher for unix](https://python-launcher.app) which also provides a `py` command (and is waaaay faster than pyenv et al). Might be worth tying it to the presence of a `py` command in the Path rather than just Windows.

---

_Comment by @T-256 on 2024-02-17 11:04_

Related https://github.com/astral-sh/uv/issues/1521

---

_Assigned to @MichaReiser by @konstin on 2024-02-19 12:56_

---

_Closed by @MichaReiser on 2024-02-22 07:47_

---
