```yaml
number: 1763
title: "uv venv creates path starting with \\\\?\\ in activate.bat which doesn't work"
type: issue
state: closed
author: orlp
labels:
  - windows
assignees: []
created_at: 2024-02-20T15:41:36Z
updated_at: 2024-02-21T15:52:34Z
url: https://github.com/astral-sh/uv/issues/1763
synced_at: 2026-01-12T15:58:32Z
```

# uv venv creates path starting with \\?\ in activate.bat which doesn't work

---

_@orlp_

To reproduce on Windows 10 with uv 0.1.5:

```
uv venv uv-venv
cd uv-venv
.\Scripts\activate.bat
where python.exe
python.exe -c "import sys; print(sys.executable)"
```

On my machine this prints:

```
(venv) D:\tmp\uv\uv-venv>where python.exe
\\?\D:\tmp\uv\uv-venv\Scripts\python.exe
C:\dev\Python312\python.exe

(venv) D:\tmp\uv\uv-venv>python.exe -c "import sys; print(sys.executable)"
C:\dev\Python312\python.exe
```

It appears that `cmd.exe` interprets a path starting with `\\?\` in the PATH variable incorrectly, despite `where` resolving it.

---

Normal `venv` does not start paths with `\\?\` in the PATH variable, and this works fine on my machine. So perhaps this could be changed?

---

_Renamed from "uv venv creates 'long path' in activate.bat which doesn't work" to "uv venv creates path starting with \\?\ in activate.bat which doesn't work" by @orlp on 2024-02-20 15:41_

---

_Label `windows` added by @MichaReiser on 2024-02-20 15:54_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-02-20 15:54_

---

_Comment by @charliermarsh on 2024-02-20 16:02_

@MichaReiser - (We have a `normalize()` trait method for this.)

---

_Closed by @MichaReiser on 2024-02-21 15:52_

---
