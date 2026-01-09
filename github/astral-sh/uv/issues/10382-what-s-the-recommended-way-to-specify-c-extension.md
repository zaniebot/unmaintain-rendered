---
number: 10382
title: "What's the recommended way to specify C-extension build?"
type: issue
state: closed
author: Guitaricet
labels:
  - question
assignees: []
created_at: 2025-01-08T01:56:12Z
updated_at: 2025-03-03T03:22:04Z
url: https://github.com/astral-sh/uv/issues/10382
synced_at: 2026-01-07T13:12:18-06:00
---

# What's the recommended way to specify C-extension build?

---

_Issue opened by @Guitaricet on 2025-01-08 01:56_

Hello! Love uv so far

I'm getting a bit confused with what's the recommended way to migrate python bindings for C-modules to uv with `setup.py` like this:

```
import numpy
from setuptools import Extension, setup

a_module = Extension(
    "my_extension",
    sources=["extension.c"],
    libraries=["turbojpeg"],
    library_dirs=["/usr/lib"],
    include_dirs=[numpy.get_include(), "/usr/include/"],
)

setup(
    name="my_extension",
    version="1.0",
    description="Extension written in C",
    ext_modules=[a_module],
    install_requires=["numpy<2.0.0"],
)
```

I think specifically, it's not obvious how to describe `Extension` object. Any recommendations would be really appreciated

---

_Comment by @samypr100 on 2025-01-08 03:06_

Howdy, this seems related to https://github.com/astral-sh/uv/issues/10267

I would use a build backend that facilitates building extension modules in a build isolation compatible manner out of the box such as [scikit-build-core](https://github.com/scikit-build/scikit-build-core). `uv init --build-backend scikit` can provide a working example if you're interested. This type of build backend will give you the mos flexibility and compatibility with uv over legacy setuptools approach.

Nonetheless, if your only option is to rely on legacy setuptools, uv should still call your legacy setup.py script using `setuptools.build_meta:__legacy__` as the build backend. You can specify this build backend on your pyproject.toml if needed, more info can be found in https://setuptools.pypa.io/en/latest/userguide/pyproject_config.html / https://setuptools.pypa.io/en/latest/userguide/ext_modules.html

---

_Label `question` added by @samypr100 on 2025-01-08 03:06_

---

_Closed by @charliermarsh on 2025-03-03 03:22_

---
