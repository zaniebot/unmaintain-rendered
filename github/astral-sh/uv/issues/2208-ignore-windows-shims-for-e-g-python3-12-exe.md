---
number: 2208
title: "Ignore Windows shims for (e.g.) `python3.12.exe`"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
created_at: 2024-03-05T17:41:56Z
updated_at: 2024-03-06T13:01:47Z
url: https://github.com/astral-sh/uv/issues/2208
synced_at: 2026-01-07T13:12:17-06:00
---

# Ignore Windows shims for (e.g.) `python3.12.exe`

---

_Issue opened by @charliermarsh on 2024-03-05 17:41_

```
AppData\Local\Temp
❯ uv --version
uv 0.1.14 (c525fdf2b 2024-03-04)

AppData\Local\Temp
❯ uv venv -p 3.12 vvv
  × failed to canonicalize path `C:\Users\...\AppData\Local\Microsoft\WindowsApps\python3.12.exe`
  ╰─▶ The file cannot be accessed by the system. (os error 1920)
```

_Originally posted by @madig in https://github.com/astral-sh/uv/issues/2105#issuecomment-1978399748_
            

---

_Label `bug` added by @charliermarsh on 2024-03-05 17:42_

---

_Label `windows` added by @charliermarsh on 2024-03-05 17:42_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-05 17:42_

---

_Referenced in [astral-sh/uv#2105](../../astral-sh/uv/issues/2105.md) on 2024-03-05 17:42_

---

_Referenced in [astral-sh/uv#2209](../../astral-sh/uv/pulls/2209.md) on 2024-03-05 17:53_

---

_Closed by @charliermarsh on 2024-03-05 18:25_

---

_Comment by @madig on 2024-03-06 13:01_

I can confirm that 0.1.15 fixes the issue :) thanks for working on it!

---

_Referenced in [astral-sh/uv#2264](../../astral-sh/uv/issues/2264.md) on 2024-03-07 06:24_

---
