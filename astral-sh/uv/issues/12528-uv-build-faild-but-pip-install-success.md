---
number: 12528
title: uv build faild but pip install success
type: issue
state: closed
author: candowu
labels:
  - needs-mre
assignees: []
created_at: 2025-03-28T12:45:30Z
updated_at: 2025-07-11T02:29:22Z
url: https://github.com/astral-sh/uv/issues/12528
synced_at: 2026-01-10T01:25:21Z
---

# uv build faild but pip install success

---

_Issue opened by @candowu on 2025-03-28 12:45_

### Summary

I initialized an empty project,
uv init xxx
cd xxx
uv venv
.venv/Script/activate

When using uv build, it reports an error:

 |-> The build backend returned an error  
  `-> Call to `setuptools.build_meta:__legacy__.get_requires_for_build_sdist` failed: expected value at line 1 column  
      1 (exit code: 0)  
      hint: This usually indicates a problem with the package or the build environment.  
Using pip install --use-pep517 --no-cache --force-reinstall . works without errors.
I want to use uv build—help me analyze where the problem lies.


### Platform

windows10

### Version

uv 0.6.9 (3d9460278 2025-03-20)

### Python version

python 3.11

---

_Label `bug` added by @candowu on 2025-03-28 12:45_

---

_Renamed from "uv build faild bug pip install success" to "uv build faild but pip install success" by @candowu on 2025-03-28 12:45_

---

_Comment by @charliermarsh on 2025-03-28 19:20_

Unfortunately I can't reproduce this. Running `uv build` on an empty project works. However, if you're trying to use `uv build`, you probably meant to do `uv init --package` rather than `uv init` (which defaults to creating an "application"): https://docs.astral.sh/uv/concepts/projects/init/#applications

---

_Label `bug` removed by @charliermarsh on 2025-03-28 19:20_

---

_Label `needs-mre` added by @charliermarsh on 2025-03-28 19:20_

---

_Comment by @charliermarsh on 2025-03-28 19:21_

Note also that `uv build` and `pip install` are not similar. You might have meant to use `uv pip install`?

---

_Comment by @candowu on 2025-03-29 12:36_

thank you very much @charliermarsh 

today, i try hatch build, it works well

PS C:\uv-build-testor-3> uv build
Building source distribution...
  × Failed to build `C:\uv-build-testor-3`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `hatchling.build.get_requires_for_build_sdist` failed: expected value at line 1 column 1 (exit code: 0)
      hint: This usually indicates a problem with the package or the build environment.
PS C:\uv-build-testor-3> 
PS C:\uv-build-testor-3> hatch build
───────────────────────────────────────────────────────────────────────────── sdist ─────────────────────────────────────────────────────────────────────────────dist\uv_build_testor_3-0.1.0.tar.gz
───────────────────────────────────────────────────────────────────────────── wheel ─────────────────────────────────────────────────────────────────────────────dist\uv_build_testor_3-0.1.0-py3-none-any.whl


here is pyproject.toml content:

[project]
name = "uv-build-testor-3"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "candowu", email = "" }
]
requires-python = ">=3.11"
dependencies = []

[project.scripts]
uv-build-testor-3 = "uv_build_testor_3:main"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

---

_Comment by @candowu on 2025-03-29 12:57_

i tried follow build-system, uv build failed,  python -m build works.

[build-system]
requires = ["setuptools>=61.0"]
build-backend = "setuptools.build_meta"


PS C:\uv-build-testor-3> uv build
Building source distribution...
running egg_info
writing src\uv_build_testor_3.egg-info\PKG-INFO
writing dependency_links to src\uv_build_testor_3.egg-info\dependency_links.txt
writing entry points to src\uv_build_testor_3.egg-info\entry_points.txt
writing top-level names to src\uv_build_testor_3.egg-info\top_level.txt
reading manifest file 'src\uv_build_testor_3.egg-info\SOURCES.txt'
writing manifest file 'src\uv_build_testor_3.egg-info\SOURCES.txt'
  × Failed to build `C:\uv-build-testor-3`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta.get_requires_for_build_sdist` failed: expected value at line 1 column 1 (exit code: 0)
      hint: This usually indicates a problem with the package or the build environment.

---

_Comment by @candowu on 2025-03-29 13:09_

uv pip install . also failed

PS C:\uv-build-testor-3> uv pip install .
Resolved 1 package in 14ms
  × Failed to build `uv-build-testor-3 @
  ├─▶ The build backend returned an error
  ╰─▶ Call to `hatchling.build.get_requires_for_build_wheel` failed: expected value at line 1 column 1 (exit code: 0)
      hint: This usually indicates a problem with the package or the build environment.
PS C:\uv-build-testor-3> 

---

_Closed by @charliermarsh on 2025-07-11 02:29_

---
