```yaml
number: 9216
title: "(🐞) `uv python` doesn't do anything or output anything"
type: issue
state: open
author: KotlinIsland
labels: []
assignees: []
created_at: 2024-11-19T06:38:55Z
updated_at: 2024-11-19T10:19:38Z
url: https://github.com/astral-sh/uv/issues/9216
synced_at: 2026-01-12T15:59:44Z
```

# (🐞) `uv python` doesn't do anything or output anything

---

_@KotlinIsland_

```
> uv python install 3.9                
> uv python install 3.9 --verbose      
DEBUG uv 0.5.2 (195f4b634 2024-11-14)
DEBUG Acquired lock for `C:\Users\AMONGUS\AppData\Roaming\uv\python`
DEBUG Found `cpython-3.9.0-windows-x86_64-none` for request `Python 3.9`
DEBUG Using request timeout of 30s
DEBUG Skipping installation of Python executables, use `--preview` to enable.
DEBUG Released lock at `C:\Users\AMONGUS\AppData\Roaming\uv\python\.lock`
```

---

_Comment by @my1e5 on 2024-11-19 10:16_

This is because uv has already found an existing Python installation on your machine that meets the requirement of 3.9.
```
DEBUG Found `cpython-3.9.0-windows-x86_64-none` for request `Python 3.9`
```
Therefore, there is nothing to do.

Maybe it would be useful to output some feedback to the user when verbose isn't used, to explain that a suitable version of Python is already present on your machine. But that's for the devs to decide.

There is also some previous discussion about this behaviour here:
* https://github.com/astral-sh/uv/issues/9031#issuecomment-2469311450

---
