```yaml
number: 10267
title: Best Practices / Official support to work with CMake FindPython module
type: issue
state: closed
author: YouJiacheng
labels:
  - question
assignees: []
created_at: 2025-01-02T01:33:27Z
updated_at: 2025-08-15T10:53:03Z
url: https://github.com/astral-sh/uv/issues/10267
synced_at: 2026-01-10T03:32:45Z
```

# Best Practices / Official support to work with CMake FindPython module

---

_Issue opened by @YouJiacheng on 2025-01-02 01:33_

[CMake FindPython](https://cmake.org/cmake/help/latest/module/FindPython.html)

It's required by [nanobind](https://nanobind.readthedocs.io/en/latest/building.html).

`pybind11` also has a [CMake FindPython mode](https://pybind11.readthedocs.io/en/stable/compiling.html#findpython-mode).

FYI, `pybind11`'s [manual build mode](https://pybind11.readthedocs.io/en/stable/compiling.html#building-manually) doesn't work out of box with `uv`, see https://github.com/astral-sh/uv/issues/10263

---

_Comment by @YouJiacheng on 2025-01-02 01:53_

Seems to be
`source .venv/bin/activate`
and
```cmake
cmake_minimum_required(VERSION 3.18...3.27)

project(my_project)

set(Python_FIND_VIRTUALENV FIRST) # This is the default.
find_package(Python 3.8 COMPONENTS Interpreter Development.Module REQUIRED)

```

---

_Comment by @zanieb on 2025-01-02 01:56_

Can you describe in a bit more detail what you're trying to do? A simple example repository reproducing your problem would be helpful.

---

_Label `question` added by @zanieb on 2025-01-02 01:56_

---

_Renamed from "[Question / Feature Request] Best Practices / Official support to work with CMake FindPython module" to "Best Practices / Official support to work with CMake FindPython module" by @zanieb on 2025-01-02 01:56_

---

_Comment by @YouJiacheng on 2025-01-02 02:02_

I'm trying to make `find_package(Python 3.8 COMPONENTS Interpreter Development.Module REQUIRED)` in CMake can find the correct python installed by `uv`. Current experiments show that activating the virtual environment is enough.

---

_Comment by @samypr100 on 2025-01-03 23:42_

Have you taken a look at  `uv init --build-backend scikit`? It has an example with scikit-build-core, cmake, and pybind which should work as expected. scikit-build-core facilitates the correct interpreter discovery for you. If you can't rely on anything besides pure cmake you'd have to configure something like you did such as `set(Python_FIND_VIRTUALENV FIRST)` or other FindPython3 settings such as find stratgegy and root dir so it finds the uv managed pythons.

---

_Closed by @charliermarsh on 2025-03-03 03:25_

---

_Comment by @kyle-basis on 2025-07-06 23:34_

@YouJiacheng I just released this today that might fit what you need https://github.com/basis-robotics/uvtarget

---

_Comment by @cclinet on 2025-08-15 10:53_

For anyone running into the same problem, here’s a tip: **activating the virtual environment is actually unnecessary. You just need to add the following in your CMake configuration:**

```CMake
set(Python_FIND_VIRTUALENV FIRST)
```


This makes CMake use the correct venv created by uv.

---
