```yaml
number: 5445
title: "Avoid enforcing EXE001 and EXE002 checks on WSL + Windows *file systems*"
type: issue
state: closed
author: bersbersbers
labels:
  - bug
  - good first issue
  - help wanted
assignees: []
created_at: 2023-06-29T18:07:40Z
updated_at: 2023-07-13T11:36:09Z
url: https://github.com/astral-sh/ruff/issues/5445
synced_at: 2026-01-10T11:09:47Z
```

# Avoid enforcing EXE001 and EXE002 checks on WSL + Windows *file systems*

---

_Issue opened by @bersbersbers on 2023-06-29 18:07_

This is a follow-up to https://github.com/astral-sh/ruff/issues/3110, and I am not sure it is valid, but I wanted to at least bring it up.

Running WSL on Windows means one can easily use a Windows file system as working directory. However, such file system do not support the executable flag, so to make *anything* executable, *everything* is considered executable (see also https://github.com/microsoft/WSL/issues/936). That triggers EXE002.

So I wonder if #3111 could be extended to ignore not only Window operation system, but also Windows file systems on Linux. One way of detecting such file systems would be by creating a new file and checking if it is executable:

```
bers@hostname:~$ touch foo
bers@hostname:~$ ll foo
-rw-r--r-- 1 bers bers 0 Jun 29 20:06 foo
bers@hostname:~$ cd /mnt/c/Code/project
bers@hostname:/mnt/c/Code/project$ touch foo
bers@hostname:/mnt/c/Code/project$ ll foo
-rwxrwxrwx 1 bers bers 0 Jun 29 20:06 foo*
```

---

_Comment by @bersbersbers on 2023-06-29 18:24_

Another way might be this:
```
bers@hostname:/mnt/c/Code/project$ df -T | grep /mnt/c
drvfs          9p       498171168 427346648  70824520  86% /mnt/c
```
but this may yield false-positives since metadata can also be enabled on these file systems.

In fact, a suggested workaround is to mount `/mnt/c` with `metadata`, which in fact allows to do `chmod -x` succesfully. 

However:
```
bers@hostname:/mnt/c/Code/project$ more /etc/wsl.conf
[automount]
options = "metadata"

bers@hostname:/mnt/c/Code/project$ ll bug.py
-rw-rw-rw- 1 bers bers 1232 Jun 29 13:37 bug.py

bers@hostname:/mnt/c/Code/project$ ruff bug.py
bug.py:1:1: EXE002 The file is executable but no shebang is present
Found 1 error.
```

I had to `rm -rf .ruff_cache/` to make it work.

---

_Label `bug` added by @charliermarsh on 2023-06-30 02:11_

---

_Label `question` added by @charliermarsh on 2023-06-30 02:11_

---

_Comment by @charliermarsh on 2023-07-01 02:34_

I think this makes sense. There's a crate we can use for this apparently: https://crates.io/crates/wsl.

---

_Label `good first issue` added by @charliermarsh on 2023-07-01 02:34_

---

_Label `help wanted` added by @charliermarsh on 2023-07-01 02:34_

---

_Label `question` removed by @charliermarsh on 2023-07-01 02:34_

---

_Closed by @charliermarsh on 2023-07-13 11:36_

---
