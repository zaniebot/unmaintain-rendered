---
number: 13198
title: uv pip puts the wrong interpreter in shebangs installed under bin
type: issue
state: open
author: Alexndrrr
labels:
  - bug
assignees: []
created_at: 2025-04-29T17:59:10Z
updated_at: 2025-04-29T18:14:24Z
url: https://github.com/astral-sh/uv/issues/13198
synced_at: 2026-01-10T01:25:30Z
---

# uv pip puts the wrong interpreter in shebangs installed under bin

---

_Issue opened by @Alexndrrr on 2025-04-29 17:59_

### Summary

Using `uv pip` to install packages in a virtual environment ends up generating invalid binaries under the bin directory, because the shebang refers to the interpreter used to create the venv, not the interpreter in the venv itself.

Example of issue:
```sh
❯ uv venv --python 3.10 _venv_app --verbose
DEBUG uv 0.6.17 (Homebrew 2025-04-25)
DEBUG Using Python request `3.10` from explicit request
DEBUG Searching for Python 3.10 in managed installations or search path
DEBUG Searching for managed installations at `~/.local/share/uv/python`
DEBUG Found `cpython-3.10.16-macos-aarch64-none` at `~/.pyenv/shims/python3.10` (first executable in the search path)
Using CPython 3.10.16 interpreter at: ~/.pyenv/versions/3.10.16/bin/python3.10
Creating virtual environment at: _venv_app
DEBUG Using base executable for virtual environment: ~/.pyenv/versions/3.10.16/bin/python3.10
Activate with: source _venv_app/bin/activate

❯ uv pip install --prefix _venv_app flake8 --verbose
DEBUG uv 0.6.17 (Homebrew 2025-04-25)
DEBUG Searching for default Python interpreter in virtual environments, managed installations, or search path
DEBUG Searching for managed installations at `~/.local/share/uv/python`
DEBUG Found `cpython-3.10.16-macos-aarch64-none` at `~/.pyenv/shims/python` (first executable in the search path)
Using CPython 3.10.16 interpreter at: ~/.pyenv/versions/3.10.16/bin/python
DEBUG Using `--prefix` directory at _venv_app
DEBUG Acquired lock for `_venv_app`
DEBUG At least one requirement is not satisfied: flake8
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.10.16
DEBUG Solving with target Python version: >=3.10.16
DEBUG Adding direct dependency: flake8*
DEBUG No cache entry for: https://pypi.org/simple/flake8/
DEBUG Searching for a compatible version of flake8 (*)
DEBUG Selecting: flake8==7.2.0 [compatible] (flake8-7.2.0-py2.py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/83/5c/0627be4c9976d56b1217cb5187b7504e7fd7d3503f8bfd312a04077bd4f7/flake8-7.2.0-py2.py3-none-any.whl.metadata
DEBUG Adding transitive dependency for flake8==7.2.0: mccabe>=0.7.0, <0.8.0
DEBUG Adding transitive dependency for flake8==7.2.0: pycodestyle>=2.13.0, <2.14.0
DEBUG Adding transitive dependency for flake8==7.2.0: pyflakes>=3.3.0, <3.4.0
DEBUG No cache entry for: https://pypi.org/simple/mccabe/
DEBUG No cache entry for: https://pypi.org/simple/pycodestyle/
DEBUG No cache entry for: https://pypi.org/simple/pyflakes/
DEBUG No cache entry for: https://files.pythonhosted.org/packages/15/40/b293a4fa769f3b02ab9e387c707c4cbdc34f073f945de0386107d4e669e6/pyflakes-3.3.2-py2.py3-none-any.whl.metadata
DEBUG Searching for a compatible version of mccabe (>=0.7.0, <0.8.0)
DEBUG Selecting: mccabe==0.7.0 [compatible] (mccabe-0.7.0-py2.py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/27/1a/1f68f9ba0c207934b35b86a8ca3aad8395a3d6dd7921c0686e23853ff5a9/mccabe-0.7.0-py2.py3-none-any.whl.metadata
WARN Skipping file for pycodestyle: pycodestyle-2.4.0-py3.6.egg
DEBUG No cache entry for: https://files.pythonhosted.org/packages/07/be/b00116df1bfb3e0bb5b45e29d604799f7b91dd861637e4d448b4e09e6a3e/pycodestyle-2.13.0-py2.py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pycodestyle (>=2.13.0, <2.14.0)
DEBUG Selecting: pycodestyle==2.13.0 [compatible] (pycodestyle-2.13.0-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of pyflakes (>=3.3.0, <3.4.0)
DEBUG Selecting: pyflakes==3.3.2 [compatible] (pyflakes-3.3.2-py2.py3-none-any.whl)
DEBUG Tried 4 versions: flake8 1, mccabe 1, pycodestyle 1, pyflakes 1
DEBUG marker environment resolution took 0.782s
Resolved 4 packages in 785ms
DEBUG Identified uncached distribution: pycodestyle==2.13.0
DEBUG Identified uncached distribution: flake8==7.2.0
DEBUG Identified uncached distribution: pyflakes==3.3.2
DEBUG Identified uncached distribution: mccabe==0.7.0
DEBUG No cache entry for: https://files.pythonhosted.org/packages/15/40/b293a4fa769f3b02ab9e387c707c4cbdc34f073f945de0386107d4e669e6/pyflakes-3.3.2-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/83/5c/0627be4c9976d56b1217cb5187b7504e7fd7d3503f8bfd312a04077bd4f7/flake8-7.2.0-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/07/be/b00116df1bfb3e0bb5b45e29d604799f7b91dd861637e4d448b4e09e6a3e/pycodestyle-2.13.0-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/27/1a/1f68f9ba0c207934b35b86a8ca3aad8395a3d6dd7921c0686e23853ff5a9/mccabe-0.7.0-py2.py3-none-any.whl
Prepared 4 packages in 229ms
Installed 4 packages in 3ms
 + flake8==7.2.0
 + mccabe==0.7.0
 + pycodestyle==2.13.0
 + pyflakes==3.3.2
DEBUG Released lock at `_venv_app/.lock`

❯ _venv_app/bin/flake8
Traceback (most recent call last):
  File "./_venv_app/bin/flake8", line 4, in <module>
    from flake8.main.cli import main
ModuleNotFoundError: No module named 'flake8'

❯ head -n 1 _venv_app/bin/flake8
#!~/.pyenv/versions/3.10.16/bin/python
```


This seems to be related to the usage of a custom `--prefix`, because if I use the default .venv directly it works:
```sh
❯ uv venv --verbose
DEBUG uv 0.6.17 (Homebrew 2025-04-25)
DEBUG No Python version file found in ancestors of working directory: ~/temp/uv-venv-python-issue
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `~/.local/share/uv/python`
DEBUG Found `cpython-3.10.16-macos-aarch64-none` at `~/.pyenv/shims/python` (first executable in the search path)
Using CPython 3.10.16 interpreter at: ~/.pyenv/versions/3.10.16/bin/python
Creating virtual environment at: .venv
DEBUG Using base executable for virtual environment: ~/.pyenv/versions/3.10.16/bin/python
Activate with: source .venv/bin/activate

❯ uv pip install --prefix .venv flake8 --verbose
DEBUG uv 0.6.17 (Homebrew 2025-04-25)
DEBUG Searching for default Python interpreter in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.10.16-macos-aarch64-none` at `~/temp/uv-venv-python-issue/.venv/bin/python3` (virtual environment)
Using CPython 3.10.16 interpreter at: .venv/bin/python3
DEBUG Using `--prefix` directory at .venv
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: flake8
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.10.16
DEBUG Solving with target Python version: >=3.10.16
DEBUG Adding direct dependency: flake8*
DEBUG Found fresh response for: https://pypi.org/simple/flake8/
DEBUG Searching for a compatible version of flake8 (*)
DEBUG Selecting: flake8==7.2.0 [compatible] (flake8-7.2.0-py2.py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/83/5c/0627be4c9976d56b1217cb5187b7504e7fd7d3503f8bfd312a04077bd4f7/flake8-7.2.0-py2.py3-none-any.whl.metadata
DEBUG Adding transitive dependency for flake8==7.2.0: mccabe>=0.7.0, <0.8.0
DEBUG Adding transitive dependency for flake8==7.2.0: pycodestyle>=2.13.0, <2.14.0
DEBUG Adding transitive dependency for flake8==7.2.0: pyflakes>=3.3.0, <3.4.0
DEBUG Found fresh response for: https://pypi.org/simple/mccabe/
DEBUG Searching for a compatible version of mccabe (>=0.7.0, <0.8.0)
DEBUG Selecting: mccabe==0.7.0 [compatible] (mccabe-0.7.0-py2.py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/pycodestyle/
DEBUG Found fresh response for: https://pypi.org/simple/pyflakes/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/27/1a/1f68f9ba0c207934b35b86a8ca3aad8395a3d6dd7921c0686e23853ff5a9/mccabe-0.7.0-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/15/40/b293a4fa769f3b02ab9e387c707c4cbdc34f073f945de0386107d4e669e6/pyflakes-3.3.2-py2.py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pycodestyle (>=2.13.0, <2.14.0)
DEBUG Selecting: pycodestyle==2.13.0 [compatible] (pycodestyle-2.13.0-py2.py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/07/be/b00116df1bfb3e0bb5b45e29d604799f7b91dd861637e4d448b4e09e6a3e/pycodestyle-2.13.0-py2.py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pyflakes (>=3.3.0, <3.4.0)
DEBUG Selecting: pyflakes==3.3.2 [compatible] (pyflakes-3.3.2-py2.py3-none-any.whl)
DEBUG Tried 4 versions: flake8 1, mccabe 1, pycodestyle 1, pyflakes 1
DEBUG marker environment resolution took 0.007s
Resolved 4 packages in 11ms
DEBUG Registry requirement already cached: pycodestyle==2.13.0
DEBUG Registry requirement already cached: flake8==7.2.0
DEBUG Registry requirement already cached: pyflakes==3.3.2
DEBUG Registry requirement already cached: mccabe==0.7.0
Installed 4 packages in 5ms
 + flake8==7.2.0
 + mccabe==0.7.0
 + pycodestyle==2.13.0
 + pyflakes==3.3.2
DEBUG Released lock at `.venv/.lock`

❯ head -n 1 .venv/bin/flake8
#!~/temp/uv-venv-python-issue/.venv/bin/python3
```

### Platform

Darwin 24.4.0 arm64

### Version

uv 0.6.17 (Homebrew 2025-04-25)

### Python version

Python 3.10.16 (via pyenv)

---

_Label `bug` added by @Alexndrrr on 2025-04-29 17:59_

---

_Comment by @charliermarsh on 2025-04-29 18:02_

Does `pip` behave differently here? In my basic testing, it seems like it does the same thing.

---

_Comment by @charliermarsh on 2025-04-29 18:03_

(What's the motivation for using `--prefix`? Why not `uv init _venv_app` and then `--python _venv_app`?)

---

_Comment by @Alexndrrr on 2025-04-29 18:14_

> Does `pip` behave differently here? In my basic testing, it seems like it does the same thing.

Hmm, I can't get pip to install it with the wrong interpreter. I've tried it with venvs called both `.venv` and `_venv_app` installed via `uv venv --seed` and `python3.10 -m venv --upgrade-deps` and it's correct every time.


> (What's the motivation for using `--prefix`? Why not `uv init _venv_app` and then `--python _venv_app`?)

I'm working with projects designed to use `pip`, not `uv`. I'm trying to swap out some of the project commands I use to use `uv` because it's significantly faster, but at this point I don't have the authority yet to change the project designs to make everyone use uv.

---
