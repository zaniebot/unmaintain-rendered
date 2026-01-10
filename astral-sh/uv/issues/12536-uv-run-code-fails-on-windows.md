---
number: 12536
title: "`uv run code` fails on Windows"
type: issue
state: open
author: Ravencentric
labels:
  - bug
  - windows
assignees: []
created_at: 2025-03-28T17:25:36Z
updated_at: 2025-12-10T18:29:53Z
url: https://github.com/astral-sh/uv/issues/12536
synced_at: 2026-01-10T01:25:21Z
---

# `uv run code` fails on Windows

---

_Issue opened by @Ravencentric on 2025-03-28 17:25_

### Summary

Reproduction:
```console
❯ which code
C:\Users\raven\AppData\Local\Programs\Microsoft VS Code\bin\code

❯ uv run code .
error: Failed to spawn: `code`
  Caused by: program not found

❯ uv run $(which code) . # Works!
```

Use case:
My use case is a `pyo3+maturin` project. `rust-analyzer` will fail if it cannot find a valid interpreter. A work around to that is activating the environment first and then launching vscode from within that:
```
❯ .venv/Scripts/activate
❯ code .
```
But I realized `uv` can handle this with `uv run code .` (or in my case, `uv run $(which code) .`)

### Platform

Windows 11 Pro 24H2 26100.3323

### Version

uv 0.6.10 (f2a2d982b 2025-03-25)

### Python version

Python 3.13.2

---

_Label `bug` added by @Ravencentric on 2025-03-28 17:25_

---

_Comment by @kompre on 2025-12-09 13:50_

Yup, same. I was surprised because running `uv run code .` on linux/wsl it works.

In my case I had to use `get-command` since `where` I think it's an alias, therefore this two syntax are equivalent:

```pwsh
uv run $(get-command code) .

uv run come.cmd . 
```

My guess is that the `.cmd` extension is what is throwing off `uv` since on a linux system is `which code` point to a file that is just `code` without extension (on my wsl), but `uv run explorer` works because it's pointing to `explorer.exe`


---

_Label `windows` added by @konstin on 2025-12-10 18:29_

---
