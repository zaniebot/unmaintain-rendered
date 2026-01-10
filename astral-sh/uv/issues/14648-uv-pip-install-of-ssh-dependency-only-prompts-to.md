---
number: 14648
title: "`uv pip install` of ssh dependency only prompts to enter the passkey in verbose mode"
type: issue
state: closed
author: bqback
labels:
  - bug
assignees: []
created_at: 2025-07-16T12:13:08Z
updated_at: 2025-07-16T12:37:41Z
url: https://github.com/astral-sh/uv/issues/14648
synced_at: 2026-01-10T01:25:47Z
---

# `uv pip install` of ssh dependency only prompts to enter the passkey in verbose mode

---

_Issue opened by @bqback on 2025-07-16 12:13_

### Summary

In my requirements file, I have a dependency linked via ssh://:
```
git+ssh://git@git.instan.ce/path/to/package.git@test-branch#egg=package_test
```
Installing requirements from this file seemed to hang up on resolving dependencies:
```sh
uv pip install -r requirements_n.txt
Using Python 3.13.3 environment at: venv13
   Updating ssh://git@git.instan.ce/path/to/package.git (test-branch)
⠦ Resolving dependencies... 
```
Googling didn't bring up much info, so I reran the command with `-v` and it properly asked me to enter my passkey:
```sh
 uv pip install -r requirements_n.txt -v          
DEBUG uv 0.6.14
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.13.3-linux-x86_64-gnu` at `/home/user/project/venv13/bin/python3` (active virtual environment)
Using Python 3.13.3 environment at: venv13
DEBUG Acquired lock for `venv13`
DEBUG At least one requirement is not satisfied: git+ssh://git@git.instan.ce/path/to/package.git@test-branch#egg=package_test
DEBUG Using request timeout of 30s
DEBUG Attempting GitHub fast path for: git+ssh://git@git.instan.ce/path/to/package.git@test-branch#egg=package_test
DEBUG Fetching source distribution from Git: ssh://git@git.instan.ce/path/to/package.git
DEBUG Acquired lock for `ssh://git@git.instan.ce/path/to/package`
DEBUG Updating Git source `ssh://git@git.instan.ce/path/to/package.git`
   Updating ssh://git@git.instan.ce/path/to/package.git (test-branch)
DEBUG Performing a Git fetch for: ssh://git@git.instan.ce/path/to/package.git
Enter passphrase for key '/home/user/.ssh/my_git_key': 
DEBUG Reset /home/user/.cache/uv/git-v0/checkouts/0712fa0d6fc24db5/f6599b7 to f6599b759a40e65e362ba6ec4ee6ceb072d0d255
    Updated ssh://git@git.instan.ce/path/to/package.git (f6599b759a40e65e362ba6ec4ee6ceb072d0d255)
...
```
after which the installation completed successfully. This doesn't feel like the intended operation mode.


### Platform

Fedora 42, Linux 6.14.4-300.fc42.x86_64 x86_64 GNU/Linux

### Version

uv-self 0.6.14

### Python version

Python 3.13.3

---

_Label `bug` added by @bqback on 2025-07-16 12:13_

---

_Comment by @jtfmumm on 2025-07-16 12:36_

This is a duplicate of #3783

---

_Closed by @jtfmumm on 2025-07-16 12:37_

---
