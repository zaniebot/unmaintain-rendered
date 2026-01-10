---
number: 10358
title: Failure to create a bash shim for CLI script with pyenv
type: issue
state: open
author: lczyk
labels:
  - question
assignees: []
created_at: 2025-01-07T12:29:06Z
updated_at: 2025-01-08T11:21:23Z
url: https://github.com/astral-sh/uv/issues/10358
synced_at: 2026-01-10T01:24:53Z
---

# Failure to create a bash shim for CLI script with pyenv

---

_Issue opened by @lczyk on 2025-01-07 12:29_

I'm using pyenv and pyenv-virtualenv for python management on macOS. I've noticed that `uv pip install` does not trigger a proper creation of a bash shim. To demonstrate I will use django which installs a client script called `django-admin`, and python 3.12.3. I've tried it with other packages with cli scripts and other python versions, and the result is the same. 

```bash
mkdir temp && cd temp
pyenv virtualenv 3.12.3 temp && pyenv local temp
pip install django
django-admin --help
```

`django-admin` script is then in `~/.pyenv/versions/3.12.3/envs/default/bin/django-admin`. When installing with `pip`, pyenv detects the cli script and creates a bash shim at `/Users/marcin/.pyenv/shims/django-admin`. If we instead do the following:

```bash
mkdir temp && cd temp
pyenv virtualenv 3.12.3 temp && pyenv local temp
pip install -U uv && uv pip install django
django-admin --help  # Unknown command: djando-admin
```

the shim is not created.

I am unsure whether this is an issue with uv, or pyenv. I will flag this up for the pyenv folks too, and we can close the issue which is not applicable.

---

```
pyenv 2.5.0 (installed with homebrew)
pyenv-virtualenv 1.2.4 (installed with homebrew)
uv 0.4.24 (b9cd54913 2024-10-17)
MacOS 15.1.1, M3
```




---

_Referenced in [pyenv/pyenv#3031](../../pyenv/pyenv/issues/3031.md) on 2025-01-07 12:30_

---

_Comment by @zanieb on 2025-01-07 18:25_

Can you share verbose uv logs? `uv pip install -v django`? It's possible it's not detecting your virtual environment?

---

_Label `question` added by @zanieb on 2025-01-07 18:25_

---

_Comment by @lczyk on 2025-01-08 11:19_

```
DEBUG uv 0.5.15 (eb6ad9a4f 2025-01-06)
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.12.3-macos-aarch64-none` at `~/.pyenv/versions/3.12.3/envs/temp/bin/python3` (active virtual environment)
Using Python 3.12.3 environment at: ~/.pyenv/versions/3.12.3/envs/temp
DEBUG Acquired lock for `~/.pyenv/versions/3.12.3/envs/temp`
DEBUG At least one requirement is not satisfied: django
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.3
DEBUG Solving with target Python version: >=3.12.3
DEBUG Adding direct dependency: django*
DEBUG Found fresh response for: https://pypi.org/simple/django/
DEBUG Searching for a compatible version of django (*)
DEBUG Selecting: django==5.1.4 [compatible] (Django-5.1.4-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/58/0b/8a4ab2c02982df4ed41e29f28f189459a7eba37899438e6bea7f39db793b/Django-5.1.4-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for django==5.1.4: asgiref>=3.8.1, <4
DEBUG Adding transitive dependency for django==5.1.4: sqlparse>=0.3.1
DEBUG Found fresh response for: https://pypi.org/simple/asgiref/
DEBUG Found installed version of asgiref==3.8.1 that satisfies >=3.8.1, <4
DEBUG Searching for a compatible version of asgiref (>=3.8.1, <4)
DEBUG Found installed version of asgiref==3.8.1 that satisfies >=3.8.1, <4
DEBUG Selecting: asgiref==3.8.1 [installed] (installed)
DEBUG Found fresh response for: https://pypi.org/simple/sqlparse/
DEBUG Found installed version of sqlparse==0.5.3 that satisfies >=0.3.1
DEBUG Searching for a compatible version of sqlparse (>=0.3.1)
DEBUG Found installed version of sqlparse==0.5.3 that satisfies >=0.3.1
DEBUG Selecting: sqlparse==0.5.3 [installed] (installed)
DEBUG Tried 3 versions: asgiref 1, django 1, sqlparse 1
DEBUG marker environment resolution took 0.138s
Resolved 3 packages in 142ms
DEBUG Requirement already installed: sqlparse==0.5.3
DEBUG Requirement already installed: asgiref==3.8.1
DEBUG Requirement already cached: django==5.1.4
DEBUG Preserving seed package: pip==24.0
DEBUG Preserving seed package: uv==0.5.15
Installed 1 package in 84ms
 + django==5.1.4
DEBUG Released lock at `~/.pyenv/versions/3.12.3/envs/temp/.lock
```

---

_Comment by @lczyk on 2025-01-08 11:21_

Looking over at pyenv though it does seem like it's on their side tbh. See [this](https://github.com/pyenv/pyenv/issues/3031) issue.

---
