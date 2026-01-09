---
number: 4494
title: "Regression of `uv pip install` in 0.2.5?"
type: issue
state: closed
author: karfau
labels:
  - question
assignees: []
created_at: 2024-06-24T22:29:42Z
updated_at: 2024-06-26T18:50:10Z
url: https://github.com/astral-sh/uv/issues/4494
synced_at: 2026-01-07T13:12:17-06:00
---

# Regression of `uv pip install` in 0.2.5?

---

_Issue opened by @karfau on 2024-06-24 22:29_

I have the following issue:

I have a setup where those `extras_require` are defined in `setup.py` but when running
`uv pip install -e '.[test]'`

it says
```
warning: The package `mypackage @ file://[...]` does not have an extra named `test`.
```
and also doesn't install any dependencies other then the package itself, not even the things in `requirements.txt`.

This is the case since v0.2.5, it works fine in 0.2.4.

Original post: https://github.com/astral-sh/uv/issues/2828#issuecomment-2185769691

I have not managed to create a full reproduction setup so far, but I got a bit closer:

The reproduction repository https://github.com/karfau/uv-install-issue contains a script `test.sh` which 

- drops the venv folder if it exists
- configures a virtual env, I tested with both python 3.8 (because that is what the original repository uses), but the behavior doesn't change with python 3.12
- installs uv in the version passed as the first argument
- installs the dependencies using `uv --verbose pip install -e '.[test]'`
- lists the content of `venv/bin` to check if the dependencies have been installed 

In the reproduction it prints the warning (since 0.2.0 if the pyproject.toml contains a `[project]` section) **and always installs all dependencies**.

So maybe the warning was leading me into a wrong direction, but in the original repository the warning is not present when running those steps using uv 0.2.4 but the warning is printed when using uv 0.2.5 and it is only installing the editable package but nothing from `install_requires` or `extra_requires`.

here is the `--verbose` output in the affected repository, if you have any idea in what direction to continue testing, let me know
```
+ uv --verbose pip install -e '.[test]'
DEBUG uv 0.2.15
DEBUG Searching for Python interpreter in system toolchains
DEBUG Found cpython 3.8.19 at `/home/.../venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.8.19 environment at venv/bin/python3
DEBUG Acquired lock for `venv`
DEBUG At least one requirement is not satisfied: file:///home/.../mypackage[test]
DEBUG Using request timeout of 30s
DEBUG Found PEP 621 metadata for /home/... in `pyproject.toml` (backend38)
DEBUG Acquired lock for `/home/.../.cache/uv/built-wheels-v3/editable/12f46ea7eac52d49`
DEBUG Preparing metadata for: mypackage @ file:///home/...
DEBUG No static `PKG-INFO` available for: mypackage @ file:///home/... (MissingPkgInfo)
DEBUG Found static `pyproject.toml` for: mypackage @ file:///home/...
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.8.19
DEBUG Adding direct dependency: mypackage*
DEBUG Adding direct dependency: mypackage[test]*
DEBUG Searching for a compatible version of mypackage[test] @ file:///home/... (*)
DEBUG Searching for a compatible version of mypackage @ file:///home/... (==1.307.0)
DEBUG Searching for a compatible version of mypackage[test] @ file:///home/... (==1.307.0)
DEBUG Tried 1 versions: mypackage 1
Resolved 1 package in 1ms
DEBUG Identified uncached requirement: mypackage @ file:///home/...
DEBUG Preserving seed package: pip==23.0.1
DEBUG Preserving seed package: setuptools==56.0.0
DEBUG Preserving seed package: uv==0.2.15
DEBUG Acquired lock for `/home/.../.cache/uv/built-wheels-v3/editable/12f46ea7eac52d49`
DEBUG Building: mypackage @ file:///home/...
INFO Ignoring empty directory
DEBUG Solving with installed Python version: 3.8.19
DEBUG Adding direct dependency: setuptools==68.2.2
DEBUG Found stale response for: https://pypi.org/simple/setuptools/
DEBUG Sending revalidation request for: https://pypi.org/simple/setuptools/
DEBUG Found not-modified response for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (==68.2.2)
DEBUG Selecting: setuptools==68.2.2 (setuptools-68.2.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/bb/26/7945080113158354380a12ce26873dd6c1ebd88d47f5bc24e2c5bb38c16a/setuptools-68.2.2-py3-none-any.whl.metadata
DEBUG Tried 1 versions: setuptools 1
DEBUG Installing in setuptools==68.2.2 in /home/.../.cache/uv/environments-v0/.tmpUhHv2P
DEBUG Requirement already cached: setuptools==68.2.2
DEBUG Installing build requirement: setuptools==68.2.2
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_editable()`
DEBUG Installing extra requirements for build backend
DEBUG Solving with installed Python version: 3.8.19
DEBUG Adding direct dependency: setuptools==68.2.2
DEBUG Adding direct dependency: wheel*
DEBUG Searching for a compatible version of setuptools (==68.2.2)
DEBUG Selecting: setuptools==68.2.2 (setuptools-68.2.2-py3-none-any.whl)
DEBUG Found stale response for: https://pypi.org/simple/wheel/
DEBUG Sending revalidation request for: https://pypi.org/simple/wheel/
DEBUG Found not-modified response for: https://pypi.org/simple/wheel/
DEBUG Searching for a compatible version of wheel (*)
DEBUG Selecting: wheel==0.43.0 (wheel-0.43.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7d/cd/d7460c9a869b16c3dd4e1e403cce337df165368c71d6af229a74699622ce/wheel-0.43.0-py3-none-any.whl.metadata
DEBUG Tried 2 versions: setuptools 1, wheel 1
DEBUG Installing in setuptools==68.2.2, wheel==0.43.0 in /home/.../.cache/uv/environments-v0/.tmpUhHv2P
DEBUG Requirement already installed: setuptools==68.2.2
DEBUG Requirement already cached: wheel==0.43.0
DEBUG Installing build requirement: wheel==0.43.0
DEBUG Calling `setuptools.build_meta:__legacy__.build_editable("/home/.../.cache/uv/built-wheels-v3/editable/12f46ea7eac52d49/atb4asxSb3HqEzyYl4kAX/.tmpngnApZ", {}, None)`
DEBUG Finished building: mypackage @ file:///home/...
Prepared 1 package in 4.15s
Installed 1 package in 0.76ms
 + mypackage==1.307.0 (from file:///home/...)
warning: The package `mypackage @ file:///home/...` does not have an extra named `test`.
```

---

_Comment by @charliermarsh on 2024-06-24 22:45_

The problem with your setup is that you have a `pyproject.toml` with a `[project]` section, but you haven't declared any of the metadata as dynamic. Per the spec, if you omit `dependencies` from the `[project]` section, that's equivalent to `project.dependencies = []`.

You should add `dynamic = ["dependencies", "optional-dependencies"]` to your `pyproject.toml`, under the `[project]` section.

---

_Label `question` added by @charliermarsh on 2024-06-24 23:29_

---

_Comment by @karfau on 2024-06-25 05:39_

@charliermarsh thx, a lot, that resolves the initial problem in the original repository.

It's still not clear to me if this is intended as a change in behavior and what [change between 0.2.4 and 0.2.5](https://github.com/astral-sh/uv/compare/0.2.4...0.2.5) causes this.
And why it behaves different in our repository than in the reproduction (were the dependencies are always installed even without this addition).

I'm happy to continue to experiment with the reproduction if I have any trace of what could be the reason, but currently I'm out of ideas (and capacity to find more ideas on my own).

---

_Comment by @charliermarsh on 2024-06-25 10:53_

It’s intended behavior because we added an optimization to read the metadata directly when it’s statically declared. And in this case, the metadata is technically statically declared (even though you want it to be dynamic).

---

_Closed by @charliermarsh on 2024-06-25 10:53_

---
