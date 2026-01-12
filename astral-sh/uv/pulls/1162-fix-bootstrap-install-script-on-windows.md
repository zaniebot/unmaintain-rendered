```yaml
number: 1162
title: Fix bootstrap install script on windows
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/fix-bootstrap-install-script-windows
created_at: 2024-01-29T08:59:11Z
updated_at: 2024-02-20T06:18:57Z
url: https://github.com/astral-sh/uv/pull/1162
synced_at: 2026-01-12T16:04:28Z
```

# Fix bootstrap install script on windows

---

_@konstin_

Windows doesn't support symlinks, doesn't use a `bin` directory and all pythons are called `python.exe`.

Note that this is still broken, `.\bin\python3.10.13` is missing its .exe extension and renaming it to `.\bin\python3.10.13.exe` makes it complain about not finding python310.dll.

---

_Review requested from @zanieb by @konstin on 2024-01-29 08:59_

---

_Comment by @konstin on 2024-01-29 09:00_

I could link the dll to the same directory, but that doesn't work with different patch versions since the dlls have only major and minor version.

---

_Comment by @zanieb on 2024-01-29 15:24_

How are we _supposed_ to register new Python versions?

---

_Comment by @konstin on 2024-01-29 16:20_

You have to register them in the windows registry ([PEP 514](https://peps.python.org/pep-0514/))

---

_Comment by @konstin on 2024-01-29 16:38_

An alternative would be a nested PATH:
* bin
  * 3.8.18
    * python.exe 
  * 3.12.1
    * python.exe 

---

_Comment by @zanieb on 2024-01-29 16:58_

How does discovery work with the nested path?

---

_Comment by @konstin on 2024-01-29 18:01_

We would have to implement this ourselves, i don't know a way that works with `which`.

I think the lack of a PATH that can be used with different versions is the main reason why `py`, the python launcher for windows, is shipped by default on windows; There's isn't really another good to launch a specific python version (without knowing the full path ahead of time)

---

_Comment by @zanieb on 2024-01-29 21:20_

> We would have to implement this ourselves

I mean I guess I'm fine with having the `Interpreter::find_executable` utility do a special scan of the `PUFFIN_PYTHON_PATH` on Windows for now?

---

_Comment by @konstin on 2024-02-03 16:20_

@zanieb Can we still merge this? Currently the script doesn't run at all (no symlinking) and this also adds pipx support

---

_@zanieb approved on 2024-02-03 16:26_

---

_Merged by @konstin on 2024-02-03 16:30_

---

_Closed by @konstin on 2024-02-03 16:30_

---

_Branch deleted on 2024-02-03 16:30_

---

_@methane reviewed on 2024-02-20 06:12_

---

_Review comment by @methane on `scripts/bootstrap/install.py`:7 on 2024-02-20 06:12_

What is this comment?
Does pipx support this?

---

_@methane reviewed on 2024-02-20 06:18_

---

_Review comment by @methane on `scripts/bootstrap/install.py`:7 on 2024-02-20 06:18_

It seems I used old pipx. Now I can ran it.
I understand this is [PEP 723](https://peps.python.org/pep-0723/).

---
