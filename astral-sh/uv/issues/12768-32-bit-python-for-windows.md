---
number: 12768
title: 32-bit Python for Windows
type: issue
state: open
author: fishsixs
labels:
  - question
assignees: []
created_at: 2025-04-09T02:09:38Z
updated_at: 2025-08-01T13:59:01Z
url: https://github.com/astral-sh/uv/issues/12768
synced_at: 2026-01-10T01:25:24Z
---

# 32-bit Python for Windows

---

_Issue opened by @fishsixs on 2025-04-09 02:09_

### Question

It seems that the python version default is x86-64, what do I do to specify the version x64 installation
```bash
cpython-3.10.11-windows-x86-none                     D:\python_32x\3.10.11\python.exe
cpython-3.9.21-windows-x86_64-none                   <download available>
cpython-3.8.20-windows-x86_64-none                   <download available>
cpython-3.8.10-windows-x86-none                      D:\python_32x\3.8.10\python.exe
cpython-3.7.9-windows-x86_64-none                    <download available>
(test-uv) PS E:\test_uv> uv python install 3.9
cpython-3.9.21-windows-x86_64-none ------------------------------ 398.38 KiB/21.18 MiB  
```

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @fishsixs on 2025-04-09 02:09_

---

_Comment by @zanieb on 2025-04-09 02:12_

`x86-64`  is 64-bit, `x86` alone is 32-bit.

---

_Comment by @fishsixs on 2025-04-09 02:18_

> `x86-64`是 64 位，单独是 32 位。`x86`

Did  install python without a specified version？

---

_Comment by @zanieb on 2025-04-09 02:20_

Sorry, what?

We default to the same platform that uv was built for, which looks like x86-64 on your machine.

---

_Comment by @fishsixs on 2025-04-09 02:24_

> 抱歉，什么？
> 
> 我们默认使用构建 uv 的同一平台，在您的机器上看起来像 x86-64。

I want to install python version 32-bit, what do I do

---

_Comment by @smart-gold-fish on 2025-07-03 07:45_

same question

---

_Comment by @zanieb on 2025-07-03 12:45_

We don't build 32-bit Python for Windows.

---

_Comment by @smart-gold-fish on 2025-07-04 04:32_


What's the problem with this? Please tell me how I can do it myself. I need 32-bit Windows COM objects in Python code, and I really liked uv.

---

_Renamed from "Installs the specified python version" to "32-bit Python for Windows" by @konstin on 2025-08-01 13:59_

---
