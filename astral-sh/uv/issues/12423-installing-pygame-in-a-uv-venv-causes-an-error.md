---
number: 12423
title: installing pygame in a uv venv causes an error but not with pip3
type: issue
state: open
author: yashranjan1
labels:
  - bug
assignees: []
created_at: 2025-03-24T10:19:27Z
updated_at: 2025-04-04T08:05:34Z
url: https://github.com/astral-sh/uv/issues/12423
synced_at: 2026-01-10T01:25:19Z
---

# installing pygame in a uv venv causes an error but not with pip3

---

_Issue opened by @yashranjan1 on 2025-03-24 10:19_

### Summary

When i try to import pygame in a uv venv after installing it on an M1 pro macbook, it fails to run a file. This is the error i see in the terminal

```
Traceback (most recent call last):
  File "/Users/yashranjan/Documents/snake/main.py", line 1, in <module>
    import pygame
  File "/Users/yashranjan/Documents/snake/.venv/lib/python3.13/site-packages/pygame/__init__.py", line 92, in <module>
    from pygame.base import *  # pylint: disable=wildcard-import; lgtm[py/polluting-import]
    ^^^^^^^^^^^^^^^^^^^^^^^^^
ImportError: dlopen(/Users/yashranjan/Documents/snake/.venv/lib/python3.13/site-packages/pygame/base.cpython-313-darwin.so, 0x0002): tried: '/Users/yashranjan/Documents/snake/.venv/lib/python3.13/site-packages/pygame/base.cpython-313-darwin.so' (mach-o file, but is an incompatible architecture (have 'x86_64', need 'arm64e' or 'arm64')), '/System/Volumes/Preboot/Cryptexes/OS/Users/yashranjan/Documents/snake/.venv/lib/python3.13/site-packages/pygame/base.cpython-313-darwin.so' (no such file), '/Users/yashranjan/Documents/snake/.venv/lib/python3.13/site-packages/pygame/base.cpython-313-darwin.so' (mach-o file, but is an incompatible architecture (have 'x86_64', need 'arm64e' or 'arm64'))
```

But it works perfectly fine if i use pip3 to install it globally. It doesnt work if i use uv to install it globally either.

The code i wrote for your reference
```
import pygame


def main() -> None:
    pygame.init()
    pygame.display.set_mode((400, 500))
    running = True

    while running:

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False


if __name__ == "__main__":
    main()
```

VERSION INFO
```
❯ python --version
Python 3.13.2

❯ uv pip freeze
pygame==2.6.1

❯ uv --version
uv 0.6.9 (3d9460278 2025-03-20)
```

### Platform

Darwin 24.3.0 arm64

### Version

uv 0.6.9 (3d9460278 2025-03-20)

### Python version

Python 3.13.2

---

_Label `bug` added by @yashranjan1 on 2025-03-24 10:19_

---

_Comment by @charliermarsh on 2025-03-24 13:18_

Hmm, does `uv cache clean`, followed by `uv pip install --reinstall pygame==2.6.1` do anything here?

---

_Comment by @yashranjan1 on 2025-03-24 13:40_

hey, thanks for the prompt response!

I tried that and it didnt work, here's exactly what i did

```
❯ source .venv/bin/activate
❯ python main.py
Traceback (most recent call last):
  File "/Users/yashranjan/Documents/snake/main.py", line 3, in <module>
    import pygame
  File "/Users/yashranjan/Documents/snake/.venv/lib/python3.13/site-packages/pygame/__init__.py", line 92, in <module>
    from pygame.base import *  # pylint: disable=wildcard-import; lgtm[py/polluting-import]
    ^^^^^^^^^^^^^^^^^^^^^^^^^
ImportError: dlopen(/Users/yashranjan/Documents/snake/.venv/lib/python3.13/site-packages/pygame/base.cpython-313-darwin.so, 0x0002): tried: '/Users/yashranjan/Documents/snake/.venv/lib/python3.13/site-packages/pygame/base.cpython-313-darwin.so' (mach-o file, but is an incompatible architecture (have 'x86_64', need 'arm64e' or 'arm64')), '/System/Volumes/Preboot/Cryptexes/OS/Users/yashranjan/Documents/snake/.venv/lib/python3.13/site-packages/pygame/base.cpython-313-darwin.so' (no such file), '/Users/yashranjan/Documents/snake/.venv/lib/python3.13/site-packages/pygame/base.cpython-313-darwin.so' (mach-o file, but is an incompatible architecture (have 'x86_64', need 'arm64e' or 'arm64'))
❯ uv cache clean
Clearing cache at: /Users/yashranjan/Library/Caches/uv
Removed 3692 files (178.6MiB)
❯ uv pip install --reinstall pygame==2.6.1
Resolved 1 package in 212ms
Prepared 1 package in 830ms
Uninstalled 1 package in 58ms
Installed 1 package in 14ms
 ~ pygame==2.6.1
❯ python main.py
Traceback (most recent call last):
  File "/Users/yashranjan/Documents/snake/main.py", line 3, in <module>
    import pygame
  File "/Users/yashranjan/Documents/snake/.venv/lib/python3.13/site-packages/pygame/__init__.py", line 92, in <module>
    from pygame.base import *  # pylint: disable=wildcard-import; lgtm[py/polluting-import]
    ^^^^^^^^^^^^^^^^^^^^^^^^^
ImportError: dlopen(/Users/yashranjan/Documents/snake/.venv/lib/python3.13/site-packages/pygame/base.cpython-313-darwin.so, 0x0002): tried: '/Users/yashranjan/Documents/snake/.venv/lib/python3.13/site-packages/pygame/base.cpython-313-darwin.so' (mach-o file, but is an incompatible architecture (have 'x86_64', need 'arm64e' or 'arm64')), '/System/Volumes/Preboot/Cryptexes/OS/Users/yashranjan/Documents/snake/.venv/lib/python3.13/site-packages/pygame/base.cpython-313-darwin.so' (no such file), '/Users/yashranjan/Documents/snake/.venv/lib/python3.13/site-packages/pygame/base.cpython-313-darwin.so' (mach-o file, but is an incompatible architecture (have 'x86_64', need 'arm64e' or 'arm64'))
```


---

_Comment by @zanieb on 2025-04-01 18:41_

What's your global Python install from? i.e., Homebrew? Python.org? Apple? Can you share `uv python find --system`?

Can you share verbose logs for the `uv pip install` command? (`-v`)

---

_Comment by @yashranjan1 on 2025-04-04 08:01_

I think i used python.org but im not too sure i dont remember tbh, sorry

```
❯ uv python find --system
/Library/Frameworks/Python.framework/Versions/3.13/bin/python3
```

ok this is strange but it worked this time ...?

```
❯ uv pip install pygame -v
DEBUG uv 0.6.9 (3d9460278 2025-03-20)
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.13.2-macos-x86_64-none` at `/Users/yashranjan/Documents/snake/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.13.2 environment at: .venv
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: pygame
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.13.2
DEBUG Solving with target Python version: >=3.13.2
DEBUG Adding direct dependency: pygame*
DEBUG Found stale response for: https://pypi.org/simple/pygame/
DEBUG Sending revalidation request for: https://pypi.org/simple/pygame/
DEBUG Found not-modified response for: https://pypi.org/simple/pygame/
DEBUG Searching for a compatible version of pygame (*)
DEBUG Selecting: pygame==2.6.1 [compatible] (pygame-2.6.1-cp313-cp313-macosx_10_13_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e1/91/718acf3e2a9d08a6ddcc96bd02a6f63c99ee7ba14afeaff2a51c987df0b9/pygame-2.6.1-cp313-cp313-macosx_10_13_x86_64.whl.metadata
DEBUG Tried 1 versions: pygame 1
DEBUG marker environment resolution took 0.118s
Resolved 1 package in 127ms
DEBUG Registry requirement already cached: pygame==2.6.1
Installed 1 package in 15ms
 + pygame==2.6.1
DEBUG Released lock at `/Users/yashranjan/Documents/snake/.venv/.lock`
```

---

_Comment by @yashranjan1 on 2025-04-04 08:05_

wait nvm sorry it didnt 
```
❯ uv venv
Using CPython 3.13.2 interpreter at: /Library/Frameworks/Python.framework/Versions/3.13/bin/python3
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
❯ source .venv/bin/activate
❯ uv pip freeze
❯ uv pip install pygame -v
DEBUG uv 0.6.9 (3d9460278 2025-03-20)
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.13.2-macos-x86_64-none` at `/Users/yashranjan/Documents/snake/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.13.2 environment at: .venv
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: pygame
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.13.2
DEBUG Solving with target Python version: >=3.13.2
DEBUG Adding direct dependency: pygame*
DEBUG Found fresh response for: https://pypi.org/simple/pygame/
DEBUG Searching for a compatible version of pygame (*)
DEBUG Selecting: pygame==2.6.1 [compatible] (pygame-2.6.1-cp313-cp313-macosx_10_13_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e1/91/718acf3e2a9d08a6ddcc96bd02a6f63c99ee7ba14afeaff2a51c987df0b9/pygame-2.6.1-cp313-cp313-macosx_10_13_x86_64.whl.metadata
DEBUG Tried 1 versions: pygame 1
DEBUG marker environment resolution took 0.017s
Resolved 1 package in 21ms
DEBUG Registry requirement already cached: pygame==2.6.1
Installed 1 package in 12ms
 + pygame==2.6.1
DEBUG Released lock at `/Users/yashranjan/Documents/snake/.venv/.lock`

❯ python snake.py
Traceback (most recent call last):
  File "/Users/yashranjan/Documents/snake/snake.py", line 4, in <module>
    import pygame
  File "/Users/yashranjan/Documents/snake/.venv/lib/python3.13/site-packages/pygame/__init__.py", line 92, in <module>
    from pygame.base import *  # pylint: disable=wildcard-import; lgtm[py/polluting-import]
    ^^^^^^^^^^^^^^^^^^^^^^^^^
ImportError: dlopen(/Users/yashranjan/Documents/snake/.venv/lib/python3.13/site-packages/pygame/base.cpython-313-darwin.so, 0x0002): tried: '/Users/yashranjan/Documents/snake/.venv/lib/python3.13/site-packages/pygame/base.cpython-313-darwin.so' (mach-o file, but is an incompatible architecture (have 'x86_64', need 'arm64e' or 'arm64')), '/System/Volumes/Preboot/Cryptexes/OS/Users/yashranjan/Documents/snake/.venv/lib/python3.13/site-packages/pygame/base.cpython-313-darwin.so' (no such file), '/Users/yashranjan/Documents/snake/.venv/lib/python3.13/site-packages/pygame/base.cpython-313-darwin.so' (mach-o file, but is an incompatible architecture (have 'x86_64', need 'arm64e' or 'arm64'))

---
