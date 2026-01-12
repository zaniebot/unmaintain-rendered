```yaml
number: 7222
title: Support building C/cython extensions
type: issue
state: closed
author: albireox
labels:
  - question
assignees: []
created_at: 2024-09-09T15:28:06Z
updated_at: 2024-10-26T15:19:31Z
url: https://github.com/astral-sh/uv/issues/7222
synced_at: 2026-01-12T15:59:11Z
```

# Support building C/cython extensions

---

_@albireox_

Does `uv` support building C/cython extensions, and if so is it documented somewhere? This would be similar to what's described here

https://setuptools.pypa.io/en/latest/userguide/ext_modules.html

Or, alternatively, would `uv` work if the `build-backend` is set to `"setuptools.build_meta"`?

In my opinion the `setuptools` `pyproject.toml` approach is somewhat lacking. In my experience compiling a C extension usually requires more than just defining the extension parameters and files with static text. A `.py` file that allows to configure things at runtime (for example depending on the OS) is much more useful. This would be similar to `poetry`'s workable but never documented 

```toml
[tool.poetry.build]
script = "build.py"
generate-setup-file = false
```

---

_Comment by @charliermarsh on 2024-09-09 15:31_

uv should work just fine to build an extension module as long as your build backend is configured to do so.

---

_Label `question` added by @zanieb on 2024-09-09 20:50_

---

_Comment by @zanieb on 2024-09-09 20:50_

cc @henryiii — not sure if you have any good ideas for how to improve this in our documentation.

---

_Comment by @albireox on 2024-09-09 20:56_

IMHO this should be something to consider if #3957 happens.

---

_Comment by @henryiii on 2024-09-09 21:10_

See https://learn.scientific-python.org/development/guides/packaging-compiled/. This should all work just fine with uv, AFAICT any valid PEP 621 backend should work, which is all of them except Poetry (and that's currently a WIP for the next release). Setuptools in the most recent release got experimental support for pyproject.toml extensions too, but it's a lot more rudimentary (just like the setup.py support) compared to scikit-build-core's CMake or meson-python's Meson usage.

> IMHO this should be something to consider if https://github.com/astral-sh/uv/issues/3957 happens.

IMO, no. Saving less than 1 second when compiling an extension would not be noticeable like it would be for making a wheel from an SDist. And requiring a Rust compiler is suddenly a much bigger ask when you must have it to install a project that can't ship pure-Python wheels. Having a way to call maturin without a Python call in the middle, perhaps, but a custom build backend that can compile extensions is a _massive_ project (and that's why CMake, Meson, Bazel, etc. exist).

As for docs, mentioning that there are compiled backends and linking to scikit-build-core, meson-python, and maturin might help? Hatching also supports binary-builds via plugins, including a scikit-build-core one.

---

_Comment by @albireox on 2024-09-09 22:15_

Thanks @henryiii I knew about `maturin` but hadn't realised there was something similar for C projects. To be honest I kind of prefer the setuptools/distutils approach of keeping this in a Python file rather than having to learn another syntax for CMake or similar, but that's just a preference.

I guess this would work fine as long as `uv` doesn't implement a build system that does "something" custom, as does Poetry for example. At that point one would need to decide which set of features one wants to keep.

Also not sure how this works at the development level. Setutools or poetry allow building the C extension in editable mode and copy the shared object inside the package. Every time one does `poetry install` the library is updated if something has changed. I don't think that would work with `uv sync`. One would need to issue a command to update the dependencies and another to keep the shared object in sync.

---

_Comment by @henryiii on 2024-09-09 22:20_

Setuptools also doesn't support multithreaded builds, IDEs, limited cross-compile support, no common things like C++ standard, no support for other libraries like Boost, etc. You basically get a bare-bones compile that you could probably do manually with hatching's local plugins, which are also in Python. But, regardless, it would also work just fine with uv.

And uv does support editable installs, and I think it's the default for this.

---

_Comment by @henryiii on 2024-09-09 22:22_

The biggest issue is I don't think output is shown by default except for `uv build`. So a compiled backend will sit for minutes showing nothing.

---

_Closed by @zanieb on 2024-10-21 21:46_

---

_Comment by @shakfu on 2024-10-25 16:02_

@zanieb @charliermarsh 

Can you please provide some examples in the documentation or on GitHub for building c extensions or cython extensions using uv.

I recently tried a hello world level case where I could build using setup.py but where `uv build` failed even when setuptools was specifies as the build backend in pyproject.toml





---

_Comment by @zanieb on 2024-10-25 16:06_

Can you open a new issue with details?

---

_Comment by @henryiii on 2024-10-25 16:06_

You can do `uv init --lib --build-backend scikit` (or `setuptools`, but there you'd have to manually set up the extensions) to get started.

---

_Comment by @henryiii on 2024-10-25 16:08_

Side comment: `uv init --build-backend scikit` doesn't actually do anything different than `uv init`, there's no build backend without `--lib` or similar. Maybe that should be an error?

---

_Comment by @shakfu on 2024-10-25 17:55_

@zanieb @henryiii 

Thanks for your reply.

I tried the following:

```sh
uv init --lib --build-backend setuptools hello
```
Inside the the created `hello` folder, I added `cython` to `build-system.requires` as follows:

```toml
[project]
name = "hello"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "me", email = "me@me.org" }
]
requires-python = ">=3.13"
dependencies = []

[build-system]
requires = ["setuptools>=61", "cython"]
build-backend = "setuptools.build_meta"
```

and add a basic `setup.py` file:

```python
from setuptools import setup
from Cython.Build import cythonize

setup(
    name="hello",
    ext_modules=cythonize("src/hello/*.pyx", include_path=["/usr/local/include"]),
)
```
... and add `src/hello/hello.pyx`:

```python
# hello.pyx
def add(int x, int y) -> int:
	return x + y

def world() -> str:
	return "HELLO WORLD!"
```

I can build the cython module using `python setup.py build`

but when I try to build using `uv build`:

```sh
% uv build
Building source distribution...
running egg_info
writing src/hello.egg-info/PKG-INFO
writing dependency_links to src/hello.egg-info/dependency_links.txt
writing top-level names to src/hello.egg-info/top_level.txt
reading manifest file 'src/hello.egg-info/SOURCES.txt'
writing manifest file 'src/hello.egg-info/SOURCES.txt'
running sdist
running egg_info
writing src/hello.egg-info/PKG-INFO
writing dependency_links to src/hello.egg-info/dependency_links.txt
writing top-level names to src/hello.egg-info/top_level.txt
reading manifest file 'src/hello.egg-info/SOURCES.txt'
writing manifest file 'src/hello.egg-info/SOURCES.txt'
running check
creating hello-0.1.0
creating hello-0.1.0/src/hello
creating hello-0.1.0/src/hello.egg-info
copying files to hello-0.1.0...
copying README.md -> hello-0.1.0
copying pyproject.toml -> hello-0.1.0
copying setup.py -> hello-0.1.0
copying src/hello/__init__.py -> hello-0.1.0/src/hello
copying src/hello/hello.c -> hello-0.1.0/src/hello
copying src/hello/py.typed -> hello-0.1.0/src/hello
copying src/hello.egg-info/PKG-INFO -> hello-0.1.0/src/hello.egg-info
copying src/hello.egg-info/SOURCES.txt -> hello-0.1.0/src/hello.egg-info
copying src/hello.egg-info/dependency_links.txt -> hello-0.1.0/src/hello.egg-info
copying src/hello.egg-info/top_level.txt -> hello-0.1.0/src/hello.egg-info
copying src/hello.egg-info/SOURCES.txt -> hello-0.1.0/src/hello.egg-info
Writing hello-0.1.0/setup.cfg
Creating tar archive
removing 'hello-0.1.0' (and everything under it)
Building wheel from source distribution...
Traceback (most recent call last):
  File "<string>", line 14, in <module>
    requires = get_requires_for_build({})
  File "~/.cache/uv/builds-v0/.tmpMscKeN/lib/python3.13/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
           ~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "~/.cache/uv/builds-v0/.tmpMscKeN/lib/python3.13/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
    self.run_setup()
    ~~~~~~~~~~~~~~^^
  File "~/.cache/uv/builds-v0/.tmpMscKeN/lib/python3.13/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
    ~~~~^^^^^^^^^^^^^^^^
  File "<string>", line 6, in <module>
    sys.path = [] + sys.path
                    ^^^^^^^^
  File "~/.cache/uv/builds-v0/.tmpMscKeN/lib/python3.13/site-packages/Cython/Build/Dependencies.py", line 1010, in cythonize
    module_list, module_metadata = create_extension_list(
                                   ~~~~~~~~~~~~~~~~~~~~~^
        module_list,
        ^^^^^^^^^^^^
    ...<4 lines>...
        language=language,
        ^^^^^^^^^^^^^^^^^^
        aliases=aliases)
        ^^^^^^^^^^^^^^^^
  File "~/.cache/uv/builds-v0/.tmpMscKeN/lib/python3.13/site-packages/Cython/Build/Dependencies.py", line 845, in create_extension_list
    for file in nonempty(sorted(extended_iglob(filepattern)), "'%s' doesn't match any files" % filepattern):
                ~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "~/.cache/uv/builds-v0/.tmpMscKeN/lib/python3.13/site-packages/Cython/Build/Dependencies.py", line 117, in nonempty
    raise ValueError(error_msg)
ValueError: 'src/hello/*.pyx' doesn't match any files
error: Build backend failed to determine requirements with `build_wheel()` (exit status: 1)
```

I do `uv add cython` which changes `pyproject.toml` to 

```
[project]
name = "hello"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "me", email = "me@me.org" }
]
requires-python = ">=3.13"
dependencies = [
    "cython>=3.0.11",
]

[build-system]
requires = ["setuptools>=61", "cython"]
build-backend = "setuptools.build_meta"
```

then `uv build` again:

```sh
% uv build
Building source distribution...
running egg_info
writing src/hello.egg-info/PKG-INFO
writing dependency_links to src/hello.egg-info/dependency_links.txt
writing requirements to src/hello.egg-info/requires.txt
writing top-level names to src/hello.egg-info/top_level.txt
reading manifest file 'src/hello.egg-info/SOURCES.txt'
writing manifest file 'src/hello.egg-info/SOURCES.txt'
running sdist
running egg_info
writing src/hello.egg-info/PKG-INFO
writing dependency_links to src/hello.egg-info/dependency_links.txt
writing requirements to src/hello.egg-info/requires.txt
writing top-level names to src/hello.egg-info/top_level.txt
reading manifest file 'src/hello.egg-info/SOURCES.txt'
writing manifest file 'src/hello.egg-info/SOURCES.txt'
running check
creating hello-0.1.0
creating hello-0.1.0/src/hello
creating hello-0.1.0/src/hello.egg-info
copying files to hello-0.1.0...
copying README.md -> hello-0.1.0
copying pyproject.toml -> hello-0.1.0
copying setup.py -> hello-0.1.0
copying src/hello/__init__.py -> hello-0.1.0/src/hello
copying src/hello/hello.c -> hello-0.1.0/src/hello
copying src/hello/py.typed -> hello-0.1.0/src/hello
copying src/hello.egg-info/PKG-INFO -> hello-0.1.0/src/hello.egg-info
copying src/hello.egg-info/SOURCES.txt -> hello-0.1.0/src/hello.egg-info
copying src/hello.egg-info/dependency_links.txt -> hello-0.1.0/src/hello.egg-info
copying src/hello.egg-info/requires.txt -> hello-0.1.0/src/hello.egg-info
copying src/hello.egg-info/top_level.txt -> hello-0.1.0/src/hello.egg-info
copying src/hello.egg-info/SOURCES.txt -> hello-0.1.0/src/hello.egg-info
Writing hello-0.1.0/setup.cfg
Creating tar archive
removing 'hello-0.1.0' (and everything under it)
Building wheel from source distribution...
Traceback (most recent call last):
  File "<string>", line 14, in <module>
    requires = get_requires_for_build({})
  File "~/.cache/uv/builds-v0/.tmpL0cVDL/lib/python3.13/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
           ~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "~/.cache/uv/builds-v0/.tmpL0cVDL/lib/python3.13/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
    self.run_setup()
    ~~~~~~~~~~~~~~^^
  File "~/.cache/uv/builds-v0/.tmpL0cVDL/lib/python3.13/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
    ~~~~^^^^^^^^^^^^^^^^
  File "<string>", line 6, in <module>
    sys.path = [] + sys.path
                    ^^^^^^^^
  File "~/.cache/uv/builds-v0/.tmpL0cVDL/lib/python3.13/site-packages/Cython/Build/Dependencies.py", line 1010, in cythonize
    module_list, module_metadata = create_extension_list(
                                   ~~~~~~~~~~~~~~~~~~~~~^
        module_list,
        ^^^^^^^^^^^^
    ...<4 lines>...
        language=language,
        ^^^^^^^^^^^^^^^^^^
        aliases=aliases)
        ^^^^^^^^^^^^^^^^
  File "~/.cache/uv/builds-v0/.tmpL0cVDL/lib/python3.13/site-packages/Cython/Build/Dependencies.py", line 845, in create_extension_list
    for file in nonempty(sorted(extended_iglob(filepattern)), "'%s' doesn't match any files" % filepattern):
                ~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "~/.cache/uv/builds-v0/.tmpL0cVDL/lib/python3.13/site-packages/Cython/Build/Dependencies.py", line 117, in nonempty
    raise ValueError(error_msg)
ValueError: 'src/hello/*.pyx' doesn't match any files
error: Build backend failed to determine requirements with `build_wheel()` (exit status: 1)
```


---

_Comment by @henryiii on 2024-10-25 17:58_

Do you have a MANIFEST.in? .pyx isn't included by setuptools by default, IIRC.

---

_Comment by @shakfu on 2024-10-25 18:12_

@henryiii  Thanks, that worked!

I added a `MANIFEST.in` file with the following line:

```text
include src/hello/*.pyx
```

and then `uv build` worked as expected, building the extension and packaging the built package as a wheel in the `dist` directory.

That's great. I think including such recipes in the `uv` documentation would be helpful for newcomers.



---

_Comment by @henryiii on 2024-10-25 18:14_

You don't have to do this if you use scikit-build-core. :)

I'm not sure if uv needs/should document every quirk of every build backend and binding tool?

Also, you don't need Cython as a dependency, just placing it in the `build-system.requires` as you did first is correct.

---

_Comment by @shakfu on 2024-10-25 18:22_

henryiii wrote:
> I'm not sure if uv needs/should document every quirk of every build backend and binding tool?

I disagree... the more you document the quirks, the easier it is to get people to use `uv`.  In any case, thanks to you at least using uv with cython is now documented here. :smile:


---

_Comment by @zanieb on 2024-10-25 18:45_

We can't possibly maintain the quirks of all the build backends — we don't maintain them and can't always update the documentation when things change.

---

_Comment by @henryiii on 2024-10-25 18:52_

FYI, here's the procedure for scikit-build-core:

```bash
uv init --lib --build-backend scikit hello
```

pyproject.toml:
```toml
[project]
name = "hello"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "Henry Schreiner", email = "henryschreineriii@gmail.com" }
]
requires-python = ">=3.7"
dependencies = []

[tool.scikit-build]
minimum-version = "build-system.requires"
build-dir = "build/{wheel_tag}"

[build-system]
requires = ["scikit-build-core>=0.10", "cython", "cython-cmake"]
build-backend = "scikit_build_core.build"
```

CMakeLists.txt:
```cmake
cmake_minimum_required(VERSION 3.21)
project(${SKBUILD_PROJECT_NAME} LANGUAGES C)

find_package(
  Python
  COMPONENTS Interpreter Development.Module
  REQUIRED)
include(UseCython)

cython_transpile(src/hello/hello.pyx LANGUAGE C OUTPUT_VARIABLE hello_c)

python_add_library(hello MODULE WITH_SOABI "${hello_c}")
install(TARGETS hello DESTINATION ${SKBUILD_PROJECT_NAME})
```

(PS: I really don't like the build-system at the bottom. Would it be acceptable to move it to the top, more like how `pyproject-fmt` would format it? Having it after not only the project section, but tool section too seems out of place, and it's never very long)

---

_Comment by @shakfu on 2024-10-26 14:50_

@zanieb wrote

> We can't possibly maintain the quirks of all the build backends — we don't maintain them and can't always update the documentation when things change.

I understand, but building c-extensions, cython, cffi, and even pybind11 and nanobind extensions are now pretty common. Having a place where recipes can be added and updated is better than not having it, especially if you are open to community PRs.


---

_Comment by @shakfu on 2024-10-26 14:55_

@henryiii @zanieb 

On a related note, the addition of something like `include src/hello/*.pyx` to `MANIFEST.in` means that all `.pyx` files must be included in the wheel if one uses `uv build`. It would be nice if this was not a necessary condition of using `uv build`


---

_Comment by @henryiii on 2024-10-26 14:56_

That's inclusion in the SDist, not wheel. Wheel inclusion is controlled by package data in setuptools.

---

_Comment by @shakfu on 2024-10-26 15:19_

> That's inclusion in the SDist, not wheel. Wheel inclusion is controlled by package data in setuptools.

Thanks very much. You are right again :smile:

As per [this page](https://setuptools.pypa.io/en/latest/userguide/datafiles.html) in the setuptools docs, If one adds the following to `pyproject.toml`, data files, like the `*.pyx` files mentioned earlier, are not included in the wheel. Note that `include-package-data` is true by default.

```ini
[tool.setuptools]
include-package-data = false
```


---
