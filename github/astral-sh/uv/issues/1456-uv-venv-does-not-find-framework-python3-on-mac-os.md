---
number: 1456
title: "`uv venv` does not find Framework python3 on Mac OS"
type: issue
state: closed
author: arsatiki
labels:
  - bug
  - duplicate
assignees: []
created_at: 2024-02-16T08:59:03Z
updated_at: 2024-02-16T16:52:27Z
url: https://github.com/astral-sh/uv/issues/1456
synced_at: 2026-01-07T13:12:16-06:00
---

# `uv venv` does not find Framework python3 on Mac OS

---

_Issue opened by @arsatiki on 2024-02-16 08:59_

I did some speed tests on `uv`. At some point, `uv venv` could not find the `python3` executable anymore. I have tried to reproduce the steps below but it is possible something is missing.

## Possible reproduction steps

1. `uv venv`
2. `source .venv/bin/activate`
3. `deactivate`
4. `rm -rf .venv`
5. `uv venv`

## What I expected to happen:

`uv` will create a new virtualenv in directory `.venv`

## What actually happens

```
% which python3
/Library/Frameworks/Python.framework/Versions/3.11/bin/python3
% uv venv
  × Neither `python` nor `python3` are in `PATH`. Is Python installed?
```

`python3` is a symlink to `python3.11` in the same directory:

```
 % ls -l /Library/Frameworks/Python.framework/Versions/3.11/bin/python3
lrwxrwxr-x  1 root  admin  10 Nov  9 17:14 /Library/Frameworks/Python.framework/Versions/3.11/bin/python3 -> python3.11
```

---

_Label `bug` added by @charliermarsh on 2024-02-16 15:27_

---

_Comment by @charliermarsh on 2024-02-16 15:27_

Thanks! Can you confirm which version of `uv` you're on? (We _may_ have fixed this in 0.1.2.)

---

_Comment by @arsatiki on 2024-02-16 16:49_

The version with the problem is 0.1.0 

I upgraded to 0.1.2 and this is indeed fixed.

---

_Comment by @MichaReiser on 2024-02-16 16:50_

Thanks for the update

---

_Closed by @MichaReiser on 2024-02-16 16:50_

---

_Label `duplicate` added by @zanieb on 2024-02-16 16:52_

---
