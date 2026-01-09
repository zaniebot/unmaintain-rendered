---
number: 1524
title: "failed to copy file from C:\\Users\\xxxxx\\.conda\\envs\\uv\\Lib\\venv\\scripts\\nt\\python.exe"
type: issue
state: closed
author: OliverKleinBST
labels:
  - bug
  - windows
assignees: []
created_at: 2024-02-16T18:45:04Z
updated_at: 2024-03-07T15:57:34Z
url: https://github.com/astral-sh/uv/issues/1524
synced_at: 2026-01-07T13:12:16-06:00
---

# failed to copy file from C:\Users\xxxxx\.conda\envs\uv\Lib\venv\scripts\nt\python.exe

---

_Issue opened by @OliverKleinBST on 2024-02-16 18:45_

Running
uv pip compile -o requirements.txt -p 3.9 requirements.in
on windows with conda installation provides error
"failed to copy file from C:\Users\xxxxx\.conda\envs\uv\Lib\venv\scripts\nt\python.exe"

When I am located in my conda environment the python.exe is located here:
C:\Users\xxxxx\.conda\envs\uv\python.exe

while there is no python.exe in the path it actually looks for it.
(uv) dir C:\Users\xxxxx\.conda\envs\uv\Lib\venv\scripts\nt
 Volume in drive C is Windows
 Directory of C:\Users\xxxxx\.conda\envs\uv\Lib\venv\scripts\nt

16/02/2024  19:30    <DIR>          .
16/02/2024  19:30    <DIR>          ..
24/08/2023  18:59               967 activate.bat
24/08/2023  18:59               368 deactivate.bat
               2 File(s)          1.335 bytes

CONDA_PREFIX is set to 
C:\Users\xxxxx\.conda\envs\uv

---

_Label `bug` added by @zanieb on 2024-02-16 18:51_

---

_Label `windows` added by @zanieb on 2024-02-16 18:51_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-07 15:57_

---

_Comment by @charliermarsh on 2024-03-07 15:57_

I think this is _probably_ fixed. If not, let me know and I'll re-open.

---

_Closed by @charliermarsh on 2024-03-07 15:57_

---
