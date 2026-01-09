---
number: 10880
title: "error: Failed to initialize cache at <dir>/.ruff_cache: File exists (os error 17)"
type: issue
state: closed
author: pjonsson
labels:
  - bug
assignees: []
created_at: 2024-04-11T14:24:04Z
updated_at: 2024-04-11T16:09:08Z
url: https://github.com/astral-sh/ruff/issues/10880
synced_at: 2026-01-07T13:12:15-06:00
---

# error: Failed to initialize cache at <dir>/.ruff_cache: File exists (os error 17)

---

_Issue opened by @pjonsson on 2024-04-11 14:24_

Using Ruff 0.3.5 on Ubuntu 22.04 LTS, I just got this error:
```
<poetry install completes>
make[1]: Leaving directory '<dir>'
15 files already formatted
error: Failed to initialize cache at <dir>/.ruff_cache: File exists (os error 17)
```
This is the first time over hundreds of runs (Ruff 0.3.4/0.3.5), so unfortunately I don't have a reproducer.

This part of the setup is easy to explain though, I'm running multiple linters in parallel, these are the relevant parts of the Makefile:
```Makefile
lint: check format <other linters>

check:
        poetry run ruff check
format:
        poetry run ruff format --check --diff
```
and then `make -j4 lint` is called. The "make" is the GNU Make version 4.x that comes as a package in Ubuntu 22.04.

The non-Ruff linters (like mypy) are explicitly passed the subdirectories `src tests` so I do not believe they would create `.ruff_cache` or anything inside Ruff's cache directory at the project top level. All linters are configured to only report errors (so no `--fix` or similar is used), so the source code is not changed underneath the linters.

I managed to get a file listing from the machine before things were cleared:
```
user@<dir> $ ls -l .ruff_cache/
total 8
drwxr-xr-x 2 gitlab-runner gitlab-runner 4096 Apr 11 13:16 0.3.5
-rw-r--r-- 1 gitlab-runner gitlab-runner   43 Apr 11 13:16 CACHEDIR.TAG
user@<dir> $ ls -l .ruff_cache/0.3.5/
total 20
-rw------- 1 gitlab-runner gitlab-runner  204 Apr 11 13:16 10942930790367313274
-rw------- 1 gitlab-runner gitlab-runner  137 Apr 11 13:16 13043565573132441774
-rw------- 1 gitlab-runner gitlab-runner  436 Apr 11 13:16 16835848346372968819
-rw------- 1 gitlab-runner gitlab-runner 1547 Apr 11 13:16 2039258722430934445
-rw------- 1 gitlab-runner gitlab-runner  844 Apr 11 13:16 262724646336492797
```
I have no idea if the directory/files are as expected, but the file permissions look as I would expect them to look, and the machine has plenty of both inodes and space available.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-11 14:47_

---

_Label `bug` added by @charliermarsh on 2024-04-11 14:47_

---

_Comment by @charliermarsh on 2024-04-11 14:47_

Thanks, probably a rare race condition in the cache.

---

_Referenced in [astral-sh/ruff#10884](../../astral-sh/ruff/pulls/10884.md) on 2024-04-11 15:54_

---

_Closed by @charliermarsh on 2024-04-11 16:09_

---
