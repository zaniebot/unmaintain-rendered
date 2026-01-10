---
number: 13068
title: System python before venv python in sys.path leading to Pillow crash.
type: issue
state: closed
author: csd4ni3l
labels:
  - question
assignees: []
created_at: 2025-04-23T13:17:43Z
updated_at: 2025-04-23T15:35:13Z
url: https://github.com/astral-sh/uv/issues/13068
synced_at: 2026-01-10T01:25:28Z
---

# System python before venv python in sys.path leading to Pillow crash.

---

_Issue opened by @csd4ni3l on 2025-04-23 13:17_

### Summary

I am on Arch Linux, with the system python being 3.13, but i wanted to use 3.11, so i installed it with `uv python install 3.11`. 

I set the python version to 3.11 for the uv project, and then ran `uv sync` which created my venv and then tried to run my project, but i had a Pillow crash from Arcade.

The problem is that for me, uv doesnt remove the system python from the sys.path, even with the only-managed flag set, the sys.path looks like this:
`['/home/csd4ni3l/Documents/Python3/EndlZ/src', '/usr/lib/python3.12/site-packages', '/usr/lib/python3.13/site-packages', '/home/csd4ni3l/Documents/Python3/EndlZ/src/$PYTHONPATH', '/home/csd4ni3l/.local/share/uv/python/cpython-3.11.11-linux-x86_64-gnu/lib/python311.zip', '/home/csd4ni3l/.local/share/uv/python/cpython-3.11.11-linux-x86_64-gnu/lib/python3.11', '/home/csd4ni3l/.local/share/uv/python/cpython-3.11.11-linux-x86_64-gnu/lib/python3.11/lib-dynload', '/home/csd4ni3l/Documents/Python3/EndlZ/src/.venv/lib/python3.11/site-packages']`

a simple uv run test.py, with test.py including this will lead to the issue:
`from PIL import Image`

the error is:
```
Traceback (most recent call last):
  File "/home/csd4ni3l/Documents/Python3/EndlZ/src/test.py", line 1, in <module>
    from PIL import Image
  File "/usr/lib/python3.13/site-packages/PIL/Image.py", line 90, in <module>
    from . import _imaging as core
ImportError: cannot import name '_imaging' from 'PIL' (/usr/lib/python3.13/site-packages/PIL/__init__.py)
```

A temporary fix for me is to remove the system python versions from sys.path with:
```
import sys
sys.path.pop(1)
sys.path.pop(1)
```

### Platform

Linux 6.14.3-arch1-1 x86_64 GNU/Linux

### Version

uv 0.6.16

### Python version

System Python is cpython-3.13.3-linux-x86_64-gnu, Venv python is cpython-3.11.11-linux-x86_64-gnu

---

_Label `bug` added by @csd4ni3l on 2025-04-23 13:17_

---

_Comment by @konstin on 2025-04-23 14:00_

Can you share the exact commands you ran that reproduce this, and the verbose logs (`-v`) for all `uv venv`/`uv sync`/etc. commands you ran? Are you setting any Python-specific environment variables?

---

_Label `bug` removed by @konstin on 2025-04-23 14:00_

---

_Label `needs-mre` added by @konstin on 2025-04-23 14:00_

---

_Comment by @csd4ni3l on 2025-04-23 14:06_

I am not setting any Python env variables.

I used:
```
uv python install cpython-3.11.11-linux-x86_64-gnu
uv sync
```

.python-version is 3.11
pyproject.toml:
```
[project]
name = "EndlZ"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "arcade",
    "pillow>=11.0.0",
    "pypresence>=4.3.0",
    "u-msgpack-python>=2.8.0",
    "websockets>=15.0",
    "zstd>=1.5.6.4",
]

[tool.uv.sources]
arcade = { git = "https://github.com/pythonarcade/arcade.git", rev = "gui/controller" }

[tool.uv]
python-preference = "only-managed"
```

To reproduce, i am going to:
```
rm -r .venv
uv sync -v
```

This results in:
```
DEBUG uv 0.6.16
DEBUG Found project root: `/home/csd4ni3l/Documents/Python3/EndlZ/src`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/home/csd4ni3l/Documents/Python3/EndlZ/src`
DEBUG Reading Python requests from version file at `/home/csd4ni3l/Documents/Python3/EndlZ/src/.python-version`
DEBUG Using Python request `3.11` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG Searching for Python 3.11 in managed installations
DEBUG Searching for managed installations at `/home/csd4ni3l/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.13.2-linux-x86_64-gnu`
DEBUG Found managed installation `cpython-3.11.11-linux-x86_64-gnu`
DEBUG Found `cpython-3.11.11-linux-x86_64-gnu` at `/home/csd4ni3l/.local/share/uv/python/cpython-3.11.11-linux-x86_64-gnu/bin/python3.11` (managed installations)
Using CPython 3.11.11
Creating virtual environment at: .venv
DEBUG Assessing Python executable as base candidate: /home/csd4ni3l/.local/share/uv/python/cpython-3.11.11-linux-x86_64-gnu/bin/python3.11
DEBUG Using base executable for virtual environment: /home/csd4ni3l/.local/share/uv/python/cpython-3.11.11-linux-x86_64-gnu/bin/python3.11
DEBUG Released lock at `/tmp/uv-eaebd82899de4845.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: endlz @ file:///home/csd4ni3l/Documents/Python3/EndlZ/src
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 14 packages in 0.83ms
DEBUG Using request timeout of 30s
DEBUG Git source requirement already cached: arcade==3.0.1 (from git+https://github.com/pythonarcade/arcade.git@fce9a5b4c4f694b84ad93a2a8b0272544662eb11)
DEBUG Registry requirement already cached: pillow==11.0.0
DEBUG Registry requirement already cached: pypresence==4.3.0
DEBUG Registry requirement already cached: u-msgpack-python==2.8.0
DEBUG Registry requirement already cached: websockets==15.0
DEBUG Registry requirement already cached: zstd==1.5.6.4
DEBUG Registry requirement already cached: pyglet==2.1.3
DEBUG Registry requirement already cached: pymunk==6.9.0
DEBUG Registry requirement already cached: pytiled-parser==2.2.9
DEBUG Registry requirement already cached: cffi==1.17.1
DEBUG Registry requirement already cached: attrs==25.1.0
DEBUG Registry requirement already cached: typing-extensions==4.12.2
DEBUG Registry requirement already cached: pycparser==2.22
Installed 13 packages in 30ms
 + arcade==3.0.1 (from git+https://github.com/pythonarcade/arcade.git@fce9a5b4c4f694b84ad93a2a8b0272544662eb11)
 + attrs==25.1.0
 + cffi==1.17.1
 + pillow==11.0.0
 + pycparser==2.22
 + pyglet==2.1.3
 + pymunk==6.9.0
 + pypresence==4.3.0
 + pytiled-parser==2.2.9
 + typing-extensions==4.12.2
 + u-msgpack-python==2.8.0
 + websockets==15.0
 + zstd==1.5.6.4
```

if you need anything else, let me know.

---

_Comment by @csd4ni3l on 2025-04-23 14:11_

I almost forgot to include, uv run logs for the test.py file:
```
DEBUG uv 0.6.16
DEBUG Found project root: `/home/csd4ni3l/Documents/Python3/EndlZ/src`
DEBUG No workspace root found, using project root
DEBUG Discovered project `endlz` at: /home/csd4ni3l/Documents/Python3/EndlZ/src
DEBUG Acquired lock for `/home/csd4ni3l/Documents/Python3/EndlZ/src`
DEBUG Reading Python requests from version file at `/home/csd4ni3l/Documents/Python3/EndlZ/src/.python-version`
DEBUG Using Python request `3.11` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version satisfies the request: `Python 3.11`
DEBUG Released lock at `/tmp/uv-eaebd82899de4845.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: endlz @ file:///home/csd4ni3l/Documents/Python3/EndlZ/src
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 14 packages in 1ms
DEBUG Using request timeout of 30s
DEBUG Requirement already installed: arcade==3.0.1 (from git+https://github.com/pythonarcade/arcade.git@fce9a5b4c4f694b84ad93a2a8b0272544662eb11)
DEBUG Requirement already installed: pillow==11.0.0
DEBUG Requirement already installed: pypresence==4.3.0
DEBUG Requirement already installed: u-msgpack-python==2.8.0
DEBUG Requirement already installed: websockets==15.0
DEBUG Requirement already installed: zstd==1.5.6.4
DEBUG Requirement already installed: pyglet==2.1.3
DEBUG Requirement already installed: pymunk==6.9.0
DEBUG Requirement already installed: pytiled-parser==2.2.9
DEBUG Requirement already installed: cffi==1.17.1
DEBUG Requirement already installed: attrs==25.1.0
DEBUG Requirement already installed: typing-extensions==4.12.2
DEBUG Requirement already installed: pycparser==2.22
Audited 13 packages in 0.09ms
DEBUG Using Python 3.11.11 interpreter at: /home/csd4ni3l/Documents/Python3/EndlZ/src/.venv/bin/python3
DEBUG Running `python test.py`
DEBUG Spawned child 54932 in process group 54929
Traceback (most recent call last):
  File "/home/csd4ni3l/Documents/Python3/EndlZ/src/test.py", line 1, in <module>
    from PIL import Image
  File "/usr/lib/python3.13/site-packages/PIL/Image.py", line 90, in <module>
    from . import _imaging as core
ImportError: cannot import name '_imaging' from 'PIL' (/usr/lib/python3.13/site-packages/PIL/__init__.py)
DEBUG Command exited with code: 1
```

---

_Comment by @csd4ni3l on 2025-04-23 14:23_

I accidentally set PYTHONPATH a long time ago because one of the packages didnt yet support 3.13. I am sorry i clogged the Issues tab.

---

_Closed by @csd4ni3l on 2025-04-23 14:23_

---

_Label `needs-mre` removed by @konstin on 2025-04-23 15:35_

---

_Label `question` added by @konstin on 2025-04-23 15:35_

---
