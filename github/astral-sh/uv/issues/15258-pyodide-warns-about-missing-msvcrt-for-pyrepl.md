---
number: 15258
title: "Pyodide warns about missing `msvcrt` for pyrepl"
type: issue
state: open
author: zanieb
labels:
  - bug
assignees: []
created_at: 2025-08-13T20:41:24Z
updated_at: 2025-08-13T20:41:24Z
url: https://github.com/astral-sh/uv/issues/15258
synced_at: 2026-01-07T13:12:19-06:00
---

# Pyodide warns about missing `msvcrt` for pyrepl

---

_Issue opened by @zanieb on 2025-08-13 20:41_

```
❯ uvx pyodide
Python 3.13.2 (main, Jul  4 2025, 13:41:45) [Clang 21.0.0git (https:/github.com/llvm/llvm-project 2f05451198e2f222ec66cec489 on emscripten
Type "help", "copyright", "credits" or "license" for more information.
warning: can't use pyrepl: No module named 'msvcrt'
>>> 
```

cc @hoodmane 

---

_Label `bug` added by @zanieb on 2025-08-13 20:41_

---
