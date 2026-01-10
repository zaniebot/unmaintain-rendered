---
number: 3837
title: uv pip install --python 3.9 does not find correct Python
type: issue
state: closed
author: aaltat
labels:
  - question
assignees: []
created_at: 2024-05-26T12:29:22Z
updated_at: 2024-05-27T16:40:18Z
url: https://github.com/astral-sh/uv/issues/3837
synced_at: 2026-01-10T01:23:31Z
---

# uv pip install --python 3.9 does not find correct Python

---

_Issue opened by @aaltat on 2024-05-26 12:29_

In GitHub actions, I install uv with pip and then run command: `uv pip install wheel --python 3.9`. Before the 0.2.0 release, this used to work fine and uv did discover Python. Now after 0.2.0 release, I see error: `error: No interpreter found for Python 3.9 in all sources` also with the latest 0.2.3 release. If I enable `--verbose` flag in the command, I see this

```bash
DEBUG Searching for Python 3.9 in all sources
DEBUG Found CPython 3.9.19 at `/opt/hostedtoolcache/Python/3.9.19/x64/bin/python3.9` (search path)
DEBUG Ignoring Python interpreter at `/opt/hostedtoolcache/Python/3.9.19/x64/bin/python3.9`: system interpreter not explicit
DEBUG Found CPython 3.9.19 at `/opt/hostedtoolcache/Python/3.9.19/x64/bin/python3` (search path)
DEBUG Ignoring Python interpreter at `/opt/hostedtoolcache/Python/3.9.19/x64/bin/python3`: system interpreter not explicit
DEBUG Found CPython 3.9.19 at `/opt/hostedtoolcache/Python/3.9.19/x64/bin/python` (search path)
DEBUG Ignoring Python interpreter at `/opt/hostedtoolcache/Python/3.9.19/x64/bin/python`: system interpreter not explicit
DEBUG Found CPython 3.9.19 at `/opt/hostedtoolcache/Python/3.9.19/x64/python` (search path)
DEBUG Ignoring Python interpreter at `/opt/hostedtoolcache/Python/3.9.19/x64/python`: system interpreter not explicit
DEBUG Found CPython 3.10.12 at `/usr/bin/python3` (search path)
DEBUG Ignoring Python interpreter at `/usr/bin/python3`: system interpreter not explicit
DEBUG Found CPython 3.10.12 at `/usr/bin/python` (search path)
DEBUG Ignoring Python interpreter at `/usr/bin/python`: system interpreter not explicit
DEBUG Found CPython 3.10.12 at `/bin/python3` (search path)
DEBUG Ignoring Python interpreter at `/bin/python3`: system interpreter not explicit
DEBUG Found CPython 3.10.12 at `/bin/python` (search path)
DEBUG Ignoring Python interpreter at `/bin/python`: system interpreter not explicit
error: No interpreter found for Python 3.9 in all sources

```
Based on the debug log, it seems to find Python3.9, but it seems that ti does not use it, which is somewhat strange. I also tried with `--python python3.9`  and with `--python python3.9.19` but it gives same results.

The following error from Github provided latest Ubuntu (https://github.com/MarketSquare/robotframework-browser/actions/runs/9243009654/job/25426578331), but I can see similar error also with Mac and Linux too. 

---

_Comment by @samypr100 on 2024-05-26 13:14_

Assuming this is CI, try adding `--system` (e.g. `uv pip install wheel --python 3.9 --system`)

---

_Comment by @zanieb on 2024-05-26 14:52_

Thanks for the report. You can either provide the full path e.g. `--system /opt/hostedtoolcache/Python/3.9.19/x64/bin/python3.9` which will opt-in to that system interpreter since it is explicitly selected or, as @samypr100 noted, use the `--system` flag. You currently can't provide _both_ the `--system` and `--python` flags, though we just enabled this in https://github.com/astral-sh/uv/pull/3830. 

~We should update the behavior when `--system` and `--python` are used together to "allow" system interpreters to be selected by version requests.~ edit: Now that we support passing both flags, you _can_ request specific criteria for the system interpreter. It should be available in the next release.

---

_Label `question` added by @zanieb on 2024-05-26 14:58_

---

_Referenced in [astral-sh/uv#3841](../../astral-sh/uv/issues/3841.md) on 2024-05-26 15:03_

---

_Comment by @zanieb on 2024-05-26 17:12_

This should be fixed in 0.2.4 which is going out right now. Let us know if you have any more questions!

---

_Closed by @zanieb on 2024-05-26 17:12_

---

_Comment by @aaltat on 2024-05-26 17:45_

So I should use `uv pip install wheel --python 3.9 —system` with 0.2.4 release 

And thank you for speedy fix 

---

_Comment by @charliermarsh on 2024-05-26 17:53_

@zanieb - Do we not allow system Pythons to be discovered with `--python 3.9`? Why not? It seems like it makes `--python 3.9` not very useful, since otherwise it can effectively only be used to validate the virtualenv? Perhaps I'm misunderstanding.

---

_Comment by @aaltat on 2024-05-26 19:08_

I did try 0.2.4 in my GitHub Actions and it seems to work with --system flag. 

Although I do agree also with previous comment.

---

_Comment by @zanieb on 2024-05-26 19:27_

@charliermarsh I don't think we should modify system environments without the `--system` flag or _explicit_ selection of the interpreter (I think specifying a version is too implicit). `--python 3.9` is still useful for things like `pip compile` and `uv venv` in which we will happily select the system interpreter without opt-in because we are not going to modify its environment. I imagine in the future we will detect multiple virtual environments or user-level environments and `--python 3.9` will be useful for mutating commands (like `pip install`) without the additional `--system` flag.

---

_Comment by @aaltat on 2024-05-26 19:46_

Well, I was too hasty, it did not work in Windows (although this looks like different issue), I run `uv pip install wheel --python 3.9  --system --verbose`. 
```cmd
DEBUG Searching for Python 3.9 in search path or `py` launcher output
DEBUG Found CPython 3.9.13 at `C:\hostedtoolcache\windows\Python\3.9.13\x64\python3.exe` (search path)
DEBUG Using Python 3.9.13 environment at C:\hostedtoolcache\windows\Python\3.9.13\x64\python3.exe
DEBUG Acquired lock for `C:\hostedtoolcache\windows\Python\3.9.13\x64`
DEBUG At least one requirement is not satisfied: wheel
DEBUG Using registry request timeout of 30s
DEBUG Solving with target Python version 3.9.13
DEBUG Adding direct dependency: wheel*
DEBUG No cache entry for: https://pypi.org/simple/wheel/
DEBUG Searching for a compatible version of wheel (*)
DEBUG Selecting: wheel==0.43.0 (wheel-0.43.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7d/cd/d7460c9a869b16c3dd4e1e403cce337df165368c71d6af229a74699622ce/wheel-0.43.0-py3-none-any.whl.metadata
DEBUG Tried 2 versions: root 1, wheel 1
Resolved 1 package in 24ms
DEBUG Identified uncached requirement: wheel==0.43.0
DEBUG Unnecessary package: argcomplete==3.3.0
DEBUG Unnecessary package: click==8.1.7
DEBUG Unnecessary package: colorama==0.4.6
DEBUG Unnecessary package: packaging==24.0
DEBUG Preserving seed package: pip==24.0
DEBUG Unnecessary package: pipx==1.5.0
DEBUG Unnecessary package: platformdirs==4.2.1
DEBUG Preserving seed package: setuptools==58.1.0
DEBUG Unnecessary package: tomli==2.0.1
DEBUG Unnecessary package: userpath==1.9.2
DEBUG Preserving seed package: uv==0.2.4
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7d/cd/d7460c9a869b16c3dd4e1e403cce337df165368c71d6af229a74699622ce/wheel-0.43.0-py3-none-any.whl
Downloaded 1 package in 161ms
Installed 1 package in 122ms
 + wheel==0.43.0
Resolved 94 packages in 11.27s
Downloaded 87 packages in 14.49s
error: failed to remove file `C:\hostedtoolcache\windows\Python\3.9.13\x64\Lib\site-packages\../../Scripts/uv.exe`
  Caused by: Access is denied. (os error 5)
Error: Process completed with exit code 1.
```

But 0.2.4 works in Linux and Mac. 

---

_Comment by @charliermarsh on 2024-05-26 20:10_

Can you please provide a full reproduction for that? The trace alone isn’t really enough. You’re trying to uninstall uv itself from the environment that it’s running in, so I need to understand how you got into that state.

---

_Comment by @aaltat on 2024-05-27 14:38_

Our project is public one: https://github.com/MarketSquare/robotframework-browser/blob/main/.github/workflows/on-push.yml#L22. I get windows latest from GitHub

Then we setup few dependencies, like NodeJS and Python (which should not affect, hopefully). After that we update pip, install uv with pip and try to install wheel with uv, where it fails. The latest try, where the last debug log was pasted is from my PR https://github.com/MarketSquare/robotframework-browser/pull/3620/files#diff-1627952414787bd2f89d526459771a1f2c92a48d7309f7b4e307f81753eedb04R46

But in practice PR does `uv pip install wheel --python 3.9  --system --verbose`. I can try to create minimalist example, if you need one.



---

_Comment by @charliermarsh on 2024-05-27 14:55_

Thanks, I'll take a look at your exact project! Unfortunately that command / flow works totally fine on my own Windows machine.

---

_Referenced in [astral-sh/uv#3865](../../astral-sh/uv/issues/3865.md) on 2024-05-27 14:55_

---

_Comment by @charliermarsh on 2024-05-27 16:40_

@aaltat -- I think I diagnosed the issue here: https://github.com/astral-sh/uv/issues/3865#issuecomment-2133815563

---
