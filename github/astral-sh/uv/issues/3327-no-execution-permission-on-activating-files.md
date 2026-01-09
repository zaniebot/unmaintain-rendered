---
number: 3327
title: no execution permission on activating files
type: issue
state: closed
author: playgithub
labels:
  - question
assignees: []
created_at: 2024-05-01T16:17:23Z
updated_at: 2024-05-01T16:24:23Z
url: https://github.com/astral-sh/uv/issues/3327
synced_at: 2026-01-07T13:12:17-06:00
---

# no execution permission on activating files

---

_Issue opened by @playgithub on 2024-05-01 16:17_

## Env
Linux 6.8.8
uv 0.1.39

## Problem
```sh
$ .venv/bin/activate
bash: .venv/bin/activate: Permission denied
```

## Reproduce
```sh
$ ls -la .venv/bin
total 48
drwxr-xr-x 2 user user 4096 May  2 00:10 .
drwxr-xr-x 4 user user 4096 May  2 00:10 ..
-rw-r--r-- 1 user user 3381 May  2 00:10 activate
-rw-r--r-- 1 user user 2260 May  2 00:10 activate.bat
-rw-r--r-- 1 user user 2648 May  2 00:10 activate.csh
-rw-r--r-- 1 user user 4211 May  2 00:10 activate.fish
-rw-r--r-- 1 user user 3896 May  2 00:10 activate.nu
-rw-r--r-- 1 user user 2786 May  2 00:10 activate.ps1
-rw-r--r-- 1 user user 2449 May  2 00:10 activate_this.py
-rw-r--r-- 1 user user 1728 May  2 00:10 deactivate.bat
-rw-r--r-- 1 user user 1215 May  2 00:10 pydoc.bat
lrwxrwxrwx 1 user user   19 May  2 00:10 python -> /usr/bin/python3.12
lrwxrwxrwx 1 user user    6 May  2 00:10 python3 -> python
lrwxrwxrwx 1 user user    6 May  2 00:10 python3.12 -> python
```

---

_Comment by @charliermarsh on 2024-05-01 16:23_

The command is `source .venv/bin/activate`.

---

_Closed by @charliermarsh on 2024-05-01 16:23_

---

_Comment by @charliermarsh on 2024-05-01 16:24_

The standard library's `venv` has the same behavior:

```
❯ python -m venv .venv
❯ .venv/bin/activate
zsh: permission denied: .venv/bin/activate
❯ source .venv/bin/activate
```

---

_Label `question` added by @charliermarsh on 2024-05-01 16:24_

---
