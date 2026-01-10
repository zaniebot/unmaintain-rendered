---
number: 1612
title: "Executables installed via `uv` cannot be run from a subprocess on Windows"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - windows
assignees: []
created_at: 2024-02-17T19:49:10Z
updated_at: 2024-02-18T11:04:11Z
url: https://github.com/astral-sh/uv/issues/1612
synced_at: 2026-01-10T01:23:08Z
---

# Executables installed via `uv` cannot be run from a subprocess on Windows

---

_Issue opened by @AlexWaygood on 2024-02-17 19:49_

Minimal repro in PowerShell:

```ps
PS C:\Users\alexw> uv venv env
Using Python 3.12.1 interpreter at C:\Users\alexw\AppData\Local\Programs\Python\Python312\python.exe
Creating virtualenv at: env
PS C:\Users\alexw> env\scripts\activate
(env) PS C:\Users\alexw> uv pip install black
Resolved 7 packages in 395ms
Downloaded 1 package in 218ms
Installed 7 packages in 106ms
 + black==24.2.0
 + click==8.1.7
 + colorama==0.4.6
 + mypy-extensions==1.0.0
 + packaging==23.2
 + pathspec==0.12.1
 + platformdirs==4.2.0
(env) PS C:\Users\alexw> python -c "import subprocess; subprocess.run(['black', 'coding/typeshed'])"
C:\Users\alexw\AppData\Local\Programs\Python\Python312\python.exe: can't open file 'C:\\Users\\alexw\\black': [Errno 2] No such file or directory
```

The same commands work fine if I use the stdlib `venv` module to create the virtual environment and I use `pip` to install black. Invoking `black` directly from the command line seems to work fine; it's only invoking it _via a subprocess_ that doesn't seem to work.

A consequence of this bug seems to be that it's impossible to run `coverage -m pytest` on Windows inside a GitHub Actions workflow if you've installed `coverage` using `uv`. Here's my attempt at switching `typeshed-stats` to use `uv`:

- https://github.com/AlexWaygood/typeshed-stats/pull/189

CI is now green everywhere, except on Windows, where invoking `coverage -m pytest` fails with the message:

```
 Error: The system cannot find the file specified.
 (from CreateProcessA(null(), child_cmdline.as_mut_ptr(), null(), null(), 1, 0,
    null(), null(), addr_of!(*si), child_process_info.as_mut_ptr()))
```

(Running `python -m coverage` works, but one test in my suite still fails, because I have a test that asserts that running `typeshed-stats` as a standalone executable works, and I invoke the standalone executable using a subprocess for _that_ test...)

---

_Label `bug` added by @AlexWaygood on 2024-02-17 19:49_

---

_Label `windows` added by @AlexWaygood on 2024-02-17 19:49_

---

_Referenced in [astral-sh/uv#1609](../../astral-sh/uv/issues/1609.md) on 2024-02-17 19:49_

---

_Comment by @MichaReiser on 2024-02-17 22:04_

I think that should be fixed by https://github.com/astral-sh/uv/pull/1523

---

_Comment by @AlexWaygood on 2024-02-17 23:24_

My local repro is now fixed -- thanks! However, I'm still seeing the strange errors over at https://github.com/AlexWaygood/typeshed-stats/pull/189 (which is now using `uv==0.1.4`). I thought those errors had the same cause as the local repro I posted above, but perhaps not.

---

_Comment by @AlexWaygood on 2024-02-18 10:58_

Since the original repro at the top of this issue is now fixed, I'll close this and open a new, more targeted issue

---

_Closed by @AlexWaygood on 2024-02-18 10:58_

---

_Comment by @AlexWaygood on 2024-02-18 11:04_

This is the new issue:
- #1639

---

_Referenced in [astral-sh/uv#1639](../../astral-sh/uv/issues/1639.md) on 2024-02-18 12:02_

---
