---
number: 12983
title: uv sync breaks venv if folder is called python
type: issue
state: closed
author: GuillaumeQuenneville
labels:
  - bug
assignees: []
created_at: 2025-04-19T16:55:43Z
updated_at: 2025-04-25T07:10:12Z
url: https://github.com/astral-sh/uv/issues/12983
synced_at: 2026-01-07T13:12:18-06:00
---

# uv sync breaks venv if folder is called python

---

_Issue opened by @GuillaumeQuenneville on 2025-04-19 16:55_

### Summary

Hi, 

I was creating python bindings for my project. I called the folder "python". I also changed the project name in the .toml and the source directories so it was not that.

When calling uv sync it breaks the the environment by removing the execute flag from the .venv/bin/activate . I also noticed that if i do the steps seperately it also breaks the python symlink.

Changing the folder name fixes the issue.

## Steps to reproduce with uv sync

```python
mkdir python && cd python
uv init
uv sync
source .venv/bin/activate
```
### Output

```bash
❯ source .venv/bin/activate
zsh: permission denied: .venv/bin/activate
❯ cat .venv/bin/activate
# Copyright (c) 2020-202x The virtualenv developers
#
# Permission is hereby granted, free of charge, to any person obtaining
... *** FILE IS FINE ***
❯ ls -al .venv/bin
total 88
drwxr-xr-x@ 14 guillaume  staff   448 19 Apr 12:35 .
drwxr-xr-x@  7 guillaume  staff   224 19 Apr 12:35 ..
-rw-r--r--@  1 guillaume  staff  4122 19 Apr 12:35 activate  <- ****execute permission is gone ****
...
```

## steps with seperate venv step
if you create a venv as a seperate step and then use uv sync it delete the symlink of python and replaces it with an invalid file.
```
mkdir python && cd python
uv init --package
uv venv
source .venv/bin/activate   # WORKS
uv sync
python 
```
### Output
```bash
❯ uv init --package
Initialized project `python`
❯ uv venv
Using CPython 3.12.8
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
❯ source .venv/bin/activate
❯ python
Python 3.12.8 (main, Dec  6 2024, 19:42:06) [Clang 18.1.8 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> ^D.   #THIS WORKS
❯ uv sync
Resolved 1 package in 7ms
      Built python @ file:///Users/guillaume/workspace/python
Prepared 1 package in 348ms
Installed 1 package in 0.96ms
 + python==0.1.0 (from file:///Users/guillaume/workspace/python)
❯ .venv/bin/python
zsh: exec format error: .venv/bin/python
❯ ls -al .venv/bin/
total 96
drwxr-xr-x@ 14 guillaume  staff   448 19 Apr 12:44 .
drwxr-xr-x@  7 guillaume  staff   224 19 Apr 12:44 ..
-rw-r--r--@  1 guillaume  staff  4122 19 Apr 12:44 activate.  <- ****Execute permission gone****
-rw-r--r--@  1 guillaume  staff  2705 19 Apr 12:44 activate.bat
-rw-r--r--@  1 guillaume  staff  2655 19 Apr 12:44 activate.csh
-rw-r--r--@  1 guillaume  staff  4219 19 Apr 12:44 activate.fish
-rw-r--r--@  1 guillaume  staff  3904 19 Apr 12:44 activate.nu
-rw-r--r--@  1 guillaume  staff  2774 19 Apr 12:44 activate.ps1
-rw-r--r--@  1 guillaume  staff  2389 19 Apr 12:44 activate_this.py
-rw-r--r--@  1 guillaume  staff  1728 19 Apr 12:44 deactivate.bat
-rw-r--r--@  1 guillaume  staff  1215 19 Apr 12:44 pydoc.bat
-rwxr-xr-x@  1 guillaume  staff   344 19 Apr 12:44 python              <- *****SYMLINK IS GONE******
lrwxr-xr-x@  1 guillaume  staff     6 19 Apr 12:44 python3 -> python
lrwxr-xr-x@  1 guillaume  staff     6 19 Apr 12:44 python3.12 -> python
```

I've only tested macos, and i've obviously renamed the folder so I'm not blocked. I did a quick search in the uv documentation and did not see a warning about this behaviour. Maybe the tool could detect the issue and make a warning also.

Thank you for your work. I love the tool.

### Platform

macos arm64

### Version

uv 0.6.14 (a4cec56dc 2025-04-09)

### Python version

python 3.12

---

_Label `bug` added by @GuillaumeQuenneville on 2025-04-19 16:55_

---

_Comment by @zanieb on 2025-04-19 20:21_

If you initialize a package, we add an entrypoint:

```
[project.scripts]
python = "python:main"
```

It matches the project name, so, in this case, it's `python` and clobbers the environment's `python`.

Yeah we can avoid adding the entrypoint to the project this case, it's pretty niche though. Broadly, we may want to warn if you have an entrypoint name that collides with an existing one — and maybe ban entrypoints called `python` entirely? I'm sort of surprised that's even allowed in the specification.

---

_Referenced in [astral-sh/uv#13051](../../astral-sh/uv/pulls/13051.md) on 2025-04-22 11:03_

---

_Closed by @konstin on 2025-04-25 07:10_

---
