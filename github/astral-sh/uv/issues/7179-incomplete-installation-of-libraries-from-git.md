---
number: 7179
title: Incomplete installation of libraries from git
type: issue
state: open
author: bheemaguli
labels:
  - needs-mre
assignees: []
created_at: 2024-09-07T19:55:14Z
updated_at: 2024-09-20T12:32:23Z
url: https://github.com/astral-sh/uv/issues/7179
synced_at: 2026-01-07T13:12:17-06:00
---

# Incomplete installation of libraries from git

---

_Issue opened by @bheemaguli on 2024-09-07 19:55_

I have a remote git repository dependency and have been installing as
`pip install -e git+https://repo@commithash#egg=foorepo` 
and the locally installed package had all files similar to the remote repo.
```
├── repo
│   ├── foorepo
│          │── subfolder
│          │    │─ file.py
│          │── setup.py
```

At the end of installatio, I was able to import function from file.py as
`from foorepo.subfolder.file import function`

But in uv, the equivalent installs like
`uv add --editable "git+https://repo@commithash"` or `uv pip install "git+https://repo@commithash"`
are cloning the repo but the subdirectories seems to be missing
```
├── repo
│   ├── foorepo
│          │── setup.py
```
and importing is raising `ModuleNotFoundError: No module named 'foorepo'`

I assumed the issue to be similar to https://github.com/astral-sh/uv/issues/1708 but none of the solutions discussed there seem to be working. Document has not been helpful in this regards.

Any idea on what the issue might be. I have tested on current (0.4.7) and few older versions of uv and issue is same across all. 

---

_Renamed from "Installing incomplete Git modules" to "Incomplete installation of libraries from git" by @bheemaguli on 2024-09-07 20:19_

---

_Comment by @bheemaguli on 2024-09-08 04:50_

I have found the issue. In the remote repo, the setup file to build the package has code as 

```
setup(
    name="repo",
    version=repo.__version__,
    packages=["foorepo"],
    include_package_data=True,
    license="MIT",
    ...
 )
 ```
 
 but uv's implementation of build or maybe setuptools is not able to recognise `packages=["foorepo"]`
 The issue can be fixed by 
```
from setuptools import setup, find_packages

setup(
    name="repo",
    version=repo.__version__,
    packages=find_packages(),
    include_package_data=True,
    license="MIT",
    ...
 )
 ```
 
 Will keep this issue open to see if this is actual issue.

---

_Comment by @notarealuseralcemy on 2024-09-19 17:41_

i'm having the same issue. the project i'm trying to install from uses no `setup.py`, only a `pyproject.toml` containing some irrelevant stuff and

```
[tool.setuptools]
packages = ["project_module"]
```

i guess something more is needed? i'm porting from pipenv and it previously worked fine with just this minimal declaration.

---

_Comment by @charliermarsh on 2024-09-19 17:43_

Unfortunately I probably won't be able to help here unless there's a reproduction that I can use.

---

_Label `needs-mre` added by @charliermarsh on 2024-09-19 17:44_

---

_Comment by @notarealuseralcemy on 2024-09-19 17:44_

okay, thanks! i'll create an mre next week

---

_Comment by @charliermarsh on 2024-09-19 18:07_

Awesome thank you!

---

_Comment by @notarealuseralcemy on 2024-09-19 18:46_

or... now. 


```shell
mkdir new_project && cd new_project
uv init
uv add "git+https://github.com/notarealuseralcemy/uv-package-mre.git"
ls .venv/lib/python*/site-packages/some_module
# > __init__.py	main.py
ls ../uv-package-mre/example_module  # or see https://github.com/notarealuseralcemy/uv-package-mre/example_module
__init__.py		main.py			some_nested_module
```
 
contents of `uv-package-mre/pyproject.toml`:
```toml
[project]
name = "uv-package-mre"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = []

[tool.setuptools]
packages = ["some_module"]
```

so you can see that `some_nested_module` is being omitted from the install.
this isn't my forte so i dunno if i'm doing something totally wrong, but as i said, there doesn't seem to be this issue with pipenv and the same setup.

---

_Comment by @notarealuseralcemy on 2024-09-20 09:27_

actually i think this issue is just caused by my misunderstanding of how the packages declaration works for setuptools, in fact it's not recursive, and each submodule should be declared explicitly? so here it should be 
```
packages = ["some_module", "some_module.some_nested_module"]
```
but i'm not sure.

i also tried 

```
[tool.setuptools.packages.find]
where = ["some_module"]
```

which is explicitly stated [here](https://setuptools.pypa.io/en/latest/userguide/package_discovery.html#finding-namespace-packages) to include submodules, but that also doesn't work.

i could eventually get it to work by adding a glob operator like:
```
[tool.setuptools.packages.find]
where = ["some_module*"]
```
but i don't think this should be required according to the setuptools docs, so i think it is an issue with uv?

---

_Comment by @bluss on 2024-09-20 12:31_

@notarealuseralcemy I saw that python -m build also excludes the files in the same way, so that confirms it is not /just/ an uv issue if it was a bug. I.e the "missing" files can be reproduced without involving uv.

---
