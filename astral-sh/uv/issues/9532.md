```yaml
number: 9532
title: uvx on old mac
type: issue
state: closed
author: Stegallo
labels: []
assignees: []
created_at: 2024-11-29T20:29:54Z
updated_at: 2024-12-04T04:22:48Z
url: https://github.com/astral-sh/uv/issues/9532
synced_at: 2026-01-10T04:36:21Z
```

# uvx on old mac

---

_Issue opened by @Stegallo on 2024-11-29 20:29_

I apologies if off topic or already answered.

I'm on an old mac with system python2.7

I do not want to modify the system python, so I'm thinking of using uv (uvx) to have a self contained environment independent from the system. I installed uv 0.5.5 (and I validated that it works perfectly if I create a venv with `uv venv .venv` and then I activate)

when I try to use uvx running the following:
`uvx --python 3.13 python -c 'print("hello world")'`

I got this error

```
  × No solution found when resolving tool dependencies:
  ╰─▶ Because python was not found in the package registry and you require python, we can conclude that your requirements are unsatisfiable.
```


is uvx not supposed to run on old systems where python2.7 is the default ?



---

_Comment by @Coruscant11 on 2024-11-29 20:59_

uvx is trying to run a module, and python is not a module I think.
Could you try `uv run --python 3.13 python -c 'print("hello world")'` ?

---

_Comment by @Stegallo on 2024-11-29 21:16_

thanks,

`uv run --python 3.13 python -c 'print("hello world")'` works perfectly.


actually also running `uvx ruff --version` works as expected

I think I got tricked by the example in the documentation where the example is
`uvx pycowsay 'hello world!'`

that gives me the following error
```

~/Library/Caches/uv/archive-v0/Q3qULlnBMDe-tYneApmmI/bin/pycowsay: line 2: realpath: command not found
~/Library/Caches/uv/archive-v0/Q3qULlnBMDe-tYneApmmI/bin/pycowsay: line 2: ~/uv/python: No such file or directory
~/Library/Caches/uv/archive-v0/Q3qULlnBMDe-tYneApmmI/bin/pycowsay: line 2: exec: ~/uv/python: cannot execute: No such file or directory
```



---

_Comment by @FishAlchemist on 2024-11-30 09:39_

@Stegallo Can you run ``uvx --verbose ruff`` and check which Python version it's using for you?

Edit:
https://docs.astral.sh/uv/concepts/python-versions/#discovery-of-python-versions
> When searching for a managed Python version, uv will prefer newer versions first. When searching for a system Python version, uv will use the first compatible version — not the newest version.
>
>If a Python version cannot be found on the system, uv will check for a compatible managed Python version download.

However, since the documentation states that it will find a compatible version, and ``uvx ruff`` requires at least Python 3.7, it should not run using Python 2.7.

pycowsay: ``python_requires=">=3.6"``.

---

_Comment by @Stegallo on 2024-11-30 18:01_

@FishAlchemist 

```
uvx --verbose ruff --version
```

```
DEBUG uv 0.5.5 (95cd8b8b3 2024-11-27)
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `~/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.13.0-macos-x86_64-none`
DEBUG Found `cpython-3.13.0-macos-x86_64-none` at `~/.local/share/uv/python/cpython-3.13.0-macos-x86_64-none/bin/python3.13` (managed installations)
DEBUG Acquired lock for `~/.local/share/uv/tools`
DEBUG Released lock at `~/.local/share/uv/tools/.lock`
DEBUG Caching via interpreter: `~/.local/share/uv/python/cpython-3.13.0-macos-x86_64-none/bin/python3.13`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.13.0
DEBUG Solving with target Python version: >=3.13.0
DEBUG Adding direct dependency: ruff*
DEBUG Found fresh response for: https://pypi.org/simple/ruff/
DEBUG Searching for a compatible version of ruff (*)
DEBUG Selecting: ruff==0.8.1 [compatible] (ruff-0.8.1-py3-none-macosx_10_12_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/89/a8/a957a8812e31facffb6a26a30be0b5b4af000a6e30c7d43a22a5232a3398/ruff-0.8.1-py3-none-macosx_10_12_x86_64.whl.metadata
DEBUG Tried 1 versions: ruff 1
DEBUG marker environment resolution took 0.015s
Resolved 1 package in 16ms
DEBUG Running `ruff --version`
DEBUG Looking at `.dist-info` at: ~/Library/Caches/uv/archive-v0/yogUHl1xqVbsxkynXGQs0/lib/python3.13/site-packages/ruff-0.8.1.dist-info
ruff 0.8.1
DEBUG Command exited with code: 0
```


however the issue still happens with a command like black

```
uvx --verbose black --version
```

```
DEBUG uv 0.5.5 (95cd8b8b3 2024-11-27)
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `~/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.13.0-macos-x86_64-none`
DEBUG Found `cpython-3.13.0-macos-x86_64-none` at `~/.local/share/uv/python/cpython-3.13.0-macos-x86_64-none/bin/python3.13` (managed installations)
DEBUG Acquired lock for `~/.local/share/uv/tools`
DEBUG Released lock at `~/.local/share/uv/tools/.lock`
DEBUG Caching via interpreter: `~/.local/share/uv/python/cpython-3.13.0-macos-x86_64-none/bin/python3.13`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.13.0
DEBUG Solving with target Python version: >=3.13.0
DEBUG Adding direct dependency: black*
DEBUG Found fresh response for: https://pypi.org/simple/black/
DEBUG Searching for a compatible version of black (*)
DEBUG Selecting: black==24.10.0 [compatible] (black-24.10.0-cp313-cp313-macosx_10_13_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d0/a0/a993f58d4ecfba035e61fca4e9f64a2ecae838fc9f33ab798c62173ed75c/black-24.10.0-cp313-cp313-macosx_10_13_x86_64.whl.metadata
DEBUG Adding transitive dependency for black==24.10.0: click>=8.0.0
DEBUG Adding transitive dependency for black==24.10.0: mypy-extensions>=0.4.3
DEBUG Adding transitive dependency for black==24.10.0: packaging>=22.0
DEBUG Adding transitive dependency for black==24.10.0: pathspec>=0.9.0
DEBUG Adding transitive dependency for black==24.10.0: platformdirs>=2
DEBUG Found fresh response for: https://pypi.org/simple/mypy-extensions/
DEBUG Found fresh response for: https://pypi.org/simple/pathspec/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/2a/e2/5d3f6ada4297caebe1a2add3b126fe800c96f56dbe5d1988a2cbe0b267aa/mypy_extensions-1.0.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/click/
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
DEBUG Searching for a compatible version of click (>=8.0.0)
DEBUG Found fresh response for: https://pypi.org/simple/platformdirs/
DEBUG Selecting: click==8.1.7 [compatible] (click-8.1.7-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/00/2e/d53fa4befbf2cfa713304affc7ca780ce4fc1fd8710527771b58311a3229/click-8.1.7-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of mypy-extensions (>=0.4.3)
DEBUG Selecting: mypy-extensions==1.0.0 [compatible] (mypy_extensions-1.0.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/ef/eb23f262cca3c0c4eb7ab1933c3b1f03d021f2c48f54763065b6f0e321be/packaging-24.2-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of packaging (>=22.0)
DEBUG Selecting: packaging==24.2 [compatible] (packaging-24.2-py3-none-any.whl)
DEBUG Searching for a compatible version of pathspec (>=0.9.0)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of platformdirs (>=2)
DEBUG Selecting: platformdirs==4.3.6 [compatible] (platformdirs-4.3.6-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/3c/a6/bc1012356d8ece4d66dd75c4b9fc6c1f6650ddd5991e421177d9f8f671be/platformdirs-4.3.6-py3-none-any.whl.metadata
DEBUG Tried 6 versions: black 1, click 1, mypy-extensions 1, packaging 1, pathspec 1, platformdirs 1
DEBUG marker environment resolution took 0.119s
Resolved 6 packages in 147ms
DEBUG Running `black --version`
DEBUG Looking at `.dist-info` at: ~/Library/Caches/uv/archive-v0/Z8sv2ZcObyjX3Wn2eFDXy/lib/python3.13/site-packages/black-24.10.0.dist-info
DEBUG Looking at `.dist-info` at: ~/Library/Caches/uv/archive-v0/Z8sv2ZcObyjX3Wn2eFDXy/lib/python3.13/site-packages/click-8.1.7.dist-info
DEBUG Looking at `.dist-info` at: ~/Library/Caches/uv/archive-v0/Z8sv2ZcObyjX3Wn2eFDXy/lib/python3.13/site-packages/mypy_extensions-1.0.0.dist-info
DEBUG Looking at `.dist-info` at: ~/Library/Caches/uv/archive-v0/Z8sv2ZcObyjX3Wn2eFDXy/lib/python3.13/site-packages/packaging-24.2.dist-info
DEBUG Looking at `.dist-info` at: ~/Library/Caches/uv/archive-v0/Z8sv2ZcObyjX3Wn2eFDXy/lib/python3.13/site-packages/pathspec-0.12.1.dist-info
DEBUG Looking at `.dist-info` at: ~/Library/Caches/uv/archive-v0/Z8sv2ZcObyjX3Wn2eFDXy/lib/python3.13/site-packages/platformdirs-4.3.6.dist-info
~/Library/Caches/uv/archive-v0/Z8sv2ZcObyjX3Wn2eFDXy/bin/black: line 2: realpath: command not found
~/Library/Caches/uv/archive-v0/Z8sv2ZcObyjX3Wn2eFDXy/bin/black: line 2: ~/uv/python: No such file or directory
~/Library/Caches/uv/archive-v0/Z8sv2ZcObyjX3Wn2eFDXy/bin/black: line 2: exec: ~/uv/python: cannot execute: No such file or directory
DEBUG Command exited with code: 126
```

---

_Comment by @FishAlchemist on 2024-11-30 18:26_

@Stegallo I just want to say it's normal not to use Python 2.7.
But didn't this problem exist in older versions of uv? Or is this actually a problem specific to Python 3.13?

---

_Comment by @Stegallo on 2024-11-30 18:40_

@FishAlchemist 
I have not tried with older versions of uv.

maybe I have a misunderstanding of the use cases for uvx.

The way I understand it is that I can use uvx if I don't want to explicit install the newest version of python (3.13) on my system for using modern python tools, especially on an old machine where python2.7 is still used by the system.


I don't think this is critical as we are talking about old machines and I have a functioning workaround.
My workaround is to use pyenv and install the version of python3 that I am interested in.


---

_Comment by @zanieb on 2024-12-03 16:00_

I think the problem is `realpath` is not available on your system.

```
❯ which realpath
/bin/realpath
```

Can you confirm?

---

_Comment by @Stegallo on 2024-12-04 04:22_

Thanks @zanieb 
yes. you are correct
```
➜  ~ which realpath
realpath not found
```

I installed it via `brew install coreutils` and now uvx works perfectly!
Thank you!


---

_Closed by @Stegallo on 2024-12-04 04:22_

---
