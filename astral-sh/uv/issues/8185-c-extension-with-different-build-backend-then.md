---
number: 8185
title: C++ extension with different build backend then rest of workspace
type: issue
state: closed
author: fabiomanz
labels:
  - question
assignees: []
created_at: 2024-10-14T19:38:30Z
updated_at: 2024-10-15T20:42:12Z
url: https://github.com/astral-sh/uv/issues/8185
synced_at: 2026-01-10T01:24:25Z
---

# C++ extension with different build backend then rest of workspace

---

_Issue opened by @fabiomanz on 2024-10-14 19:38_

Hi,

I have a Python project which uses hatchling as its default build system. I also have a C++ extension module in the same repo, which I included as a dependency in the root project of the workspace. The C++ module uses CMake with pybind11. Therefore in the pyproject.toml file of this extension, I specified the build system of the extension differently as follows:
```
[build-system]
requires = ["scikit-build-core", "pybind11"]
build-backend = "scikit_build_core.build"
```

I would expect that the extension will be build with scikit-build-core. But the build doesn't build anything on the C++ side.

Is this expected and am I doing something wrong completely or is this a bug?

---

_Comment by @zanieb on 2024-10-14 20:10_

Can you share a minimum example? It sounds like the child project should be built with the build system as you've defined it. Is a previous build cached?

---

_Label `question` added by @zanieb on 2024-10-14 20:10_

---

_Comment by @samypr100 on 2024-10-15 02:53_

https://github.com/astral-sh/uv/pull/7857 might help since it provides a template for scikit-build-core, is there more that you can share about your setup?

---

_Comment by @konstin on 2024-10-15 09:22_

For comparison, does the package build correct when installing `build` and running `python -m build`?

---

_Comment by @fabiomanz on 2024-10-15 15:21_

Thanks for the help so far. 
I can provide some more context about my setup. I have a codebase constsing of 90% Python code. It is a PySide6 GUI. But I need to interface some hardware which can only be done from C++. So this is why I have some hardware support package which uses pybind11 to allow Python to call into the C++ functions. This is a seperate member in my uv workspace. As this mostly is a Python project, I wanted to stick to `hatchling` as my build system and only use another one for the C++ library (workspace member). All these are in one repo. In the root pyproject.toml, I have added the other folder as a dependency. The C++ library only has a CMakeLists.txt file, the C++ code and another pyproject.toml.

I think `scikit-build-core` is better suited if I wouldn't have a mixed but only a C++ library, correct? 

Is specifiying a different build system in the pyproject.toml files ok and is it correct that I want two different build systems? Do I have to setup anything else to let uv know that this module needs to be built?
This is my library `pyproject.toml`:
```
[project]
name = "myhardwaremodule"
version = "0.1.0"
description = "..."
readme = "README.md"
requires-python = ">= 3.12"

[build-system]
requires = ["scikit-build-core", "pybind11"]
build-backend = "scikit_build_core.build"

```
Using the `build` module also doesn't build the library, so I seem to have something configured wrongly, but it also doesn't know about the workspace details.
I will try to get a reproducible minimal example later, but first I would like to hear if my approach is going in the right direction.

---

_Comment by @konstin on 2024-10-15 15:28_

> Is specifiying a different build system in the pyproject.toml files ok and is it correct that I want two different build systems?

Yes

> Do I have to setup anything else to let uv know that this module needs to be built?

No, that's entirely in the hand of the build backend. The way it works (through PEP 517) is that first, uv (or `python -m build`, or pip, etc.) installs the necessary packages from `build-system.requires`. It then tells hatchling, scikit-build-core or any other configured build backend the location of the project and the location for the final artifact (source distribution or wheel), everything else happens in the build backend without uv being involved.

Your approach with two packages, one with scikit-build-core and one with hatchling, sounds right from your description. You will probably get better specific help for your build problem through scikit-build-core; We can't say much from uv's side.

---

_Comment by @fabiomanz on 2024-10-15 20:42_

Thank you for your helpful comment. You guided me into the right direction and I figured it out. For others with the same problem, it was a wronlgy configred path in CMakeLists.txt. Sorry to bother the `uv` maintainers, it wasn't something related to your great tool :)

---

_Closed by @fabiomanz on 2024-10-15 20:42_

---
