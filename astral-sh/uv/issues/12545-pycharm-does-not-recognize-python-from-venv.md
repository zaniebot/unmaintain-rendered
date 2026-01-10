---
number: 12545
title: Pycharm does not recognize python from .venv
type: issue
state: closed
author: soldatov-ss
labels:
  - question
assignees: []
created_at: 2025-03-29T13:58:07Z
updated_at: 2025-07-08T13:01:28Z
url: https://github.com/astral-sh/uv/issues/12545
synced_at: 2026-01-10T01:25:21Z
---

# Pycharm does not recognize python from .venv

---

_Issue opened by @soldatov-ss on 2025-03-29 13:58_

### Question

![Image](https://github.com/user-attachments/assets/52e36c9c-8ca6-4170-a989-99798a9f2490)

I'm struggling with setting up interpreter in Pycarm, it just does not see the python from .venv that had been created by uv.




Also I don't understand why do I get permission denied on my .venv (created by uv)


```
user@laptop:~/Projects/referral-wave$ uv venv --verbose
DEBUG uv 0.6.8
DEBUG Found project root: `/home/user/Projects/referral-wave`
DEBUG No workspace root found, using project root
DEBUG Reading Python requests from version file at `/home/user/Projects/referral-wave/.python-version`
DEBUG Using Python request `cpython-3.13.2-linux-x86_64-gnu` from version file at `.python-version`
DEBUG Searching for cpython-3.13.2-linux-x86_64-gnu in managed installations or search path
DEBUG Searching for managed installations at `/home/user/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.13.2-linux-x86_64-gnu`
DEBUG Found `cpython-3.13.2-linux-x86_64-gnu` at `/home/user/.local/share/uv/python/cpython-3.13.2-linux-x86_64-gnu/bin/python3.13` (managed installations)
Using CPython 3.13.2
Creating virtual environment at: .venv
DEBUG Assessing Python executable as base candidate: /home/user/.local/share/uv/python/cpython-3.13.2-linux-x86_64-gnu/bin/python3.13
DEBUG Using base executable for virtual environment: /home/user/.local/share/uv/python/cpython-3.13.2-linux-x86_64-gnu/bin/python3.13
DEBUG Removing existing directory
uv::venv::creation

  × Failed to create virtualenv
  ╰─▶ failed to remove directory `/home/user/Projects/referral-wave/.venv`: Permission denied (os error 13)

```


I did not changed and not assigned any permissions to uv after installation

```
user@laptop:~/Projects/referral-wave$ ls -la .venv
итого 32
drwxr-xr-x  5 root root 4096 мар 29 10:59 .
drwxrwxr-x 13 user user 4096 мар 29 11:08 ..
drwxr-xr-x  2 root root 4096 мар 29 10:59 bin
-rw-r--r--  1 root root   43 мар 29 10:59 CACHEDIR.TAG
-rw-r--r--  1 root root    1 мар 29 10:59 .gitignore
drwxr-xr-x  3 root root 4096 мар 29 10:59 lib
lrwxrwxrwx  1 root root    3 мар 29 10:59 lib64 -> lib
-rw-r--r--  1 root root  141 мар 29 10:59 pyvenv.cfg
drwxr-xr-x  3 root root 4096 мар 29 10:59 share
user@laptop:~/Projects/referral-wave$ 
```

### Platform

LInux Mint (cinnamon 21.3)

### Version

0.6.8

---

_Label `question` added by @soldatov-ss on 2025-03-29 13:58_

---

_Comment by @soldatov-ss on 2025-03-29 17:02_

sorry guys, 
I resolved this, idk why, but I tried to configure this for a while and it didn't work

here's how I managed this:
```
sudo rm -rf .venv
uv venv
source .venv/bin/activate
uv sync --all-groups

then go to the pycharm and select existing interpreter

![Image](https://github.com/user-attachments/assets/fef4e58e-b6d1-4244-aa5d-39e2390e3fe1)

Idk why, but It started working for **only after** adding  `--all-groups`
```

---

_Closed by @soldatov-ss on 2025-03-29 17:02_

---

_Comment by @hendriks73 on 2025-07-08 13:01_

I believe, I have encountered the same issue. The "fix" did not help me, but at some point I have realized that one can click on the folder symbol and manually select the python executable. I would have expected PyCharm to find this itself.

---
