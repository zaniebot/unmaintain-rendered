```yaml
number: 5004
title: uv building of casadi fails
type: issue
state: closed
author: andiradulescu
labels:
  - question
assignees: []
created_at: 2024-07-12T08:12:08Z
updated_at: 2024-07-13T08:31:46Z
url: https://github.com/astral-sh/uv/issues/5004
synced_at: 2026-01-10T05:31:37Z
```

# uv building of casadi fails

---

_Issue opened by @andiradulescu on 2024-07-12 08:12_

When trying to build [casadi](https://github.com/casadi/casadi) with `uv` the build fails with:
```
CMake Error at swig/python/CMakeLists.txt:32 (file):
  file STRINGS file
  "/home/andiradulescu/.cache/uv/builds-v0/.tmpJ6W54S/include/patchlevel.h"
  cannot be read.
Call Stack (most recent call first):
  swig/python/CMakeLists.txt:62 (CasadiGetPythonVersionMajor)
```
This is because `/home/andiradulescu/.cache/uv/builds-v0/.tmpJ6W54S/` folder doesn't seem to exist:
```
ls: cannot access '/home/andiradulescu/.cache/uv/builds-v0/.tmpJ6W54S/': No such file or directory
```

CasADi uses this in it's [setup.py](https://github.com/casadi/casadi/blob/72764dc73eab7b935db7e302278508855815883b/swig/python/setup.py.in#L50C13-L50C60):
```
'-DPYTHON_INCLUDE_DIR=' + self.include_dirs[0],
```


Debugging this further, self.include_dirs has (for `uv pip install`):
```
[
'/home/andiradulescu/.cache/uv/builds-v0/.tmpaqg9V9/include',
'/home/andiradulescu/.pyenv/versions/3.12.4/include/python3.12'
]
```

Where `pip install` - that builds successfully has:
```
[
'/home/andiradulescu/.pyenv/versions/3.12.4/include/python3.12'
]
```

I tested all of this by:
- downloading the latest casadi [source](https://github.com/casadi/casadi/releases/download/3.6.5/casadi-source-v3.6.5.zip) files, so that I can also be able to add logs
- creating a venv - `uv venv`
- running either `uv pip install --verbose .` or `pip install --verbose .`

---

_Comment by @charliermarsh on 2024-07-12 13:23_

> This is because `/home/andiradulescu/.cache/uv/builds-v0/.tmpJ6W54S/` folder doesn't seem to exist:

This may be a red herring. That directory is cleaned up and deleted when the program exits, so it won't exist after the failure.

Does the package install successfully with `pip install --use-pep517`?


---

_Label `question` added by @charliermarsh on 2024-07-12 13:23_

---

_Comment by @andiradulescu on 2024-07-12 15:26_

> Does the package install successfully with `pip install --use-pep517`?

Yes, builds fine. Doesn't seem to be a difference.
```
Successfully built casadi
Installing collected packages: numpy, casadi
Successfully installed casadi-3.6.5 numpy-2.0.0
```

---

_Comment by @andiradulescu on 2024-07-12 15:34_

Also, trying to list the contents of `self.include_dirs[0]` in casadi setup.py `build_extension`, while using `uv pip install` still complains it doesn't exist:

```
error: [Errno 2] No such file or directory: '/Users/andiradulescu/Library/Caches/uv/builds-v0/.tmpYVB1tp/include'
```

But listing the parent dir ('/Users/andiradulescu/Library/Caches/uv/builds-v0/.tmpYVB1tp'), the tmp folder exits, but no `include` dir:
```
bin
get_requires_for_build_wheel.txt
pyvenv.cfg
CACHEDIR.TAG
.gitignore
lib
```

---

_Comment by @charliermarsh on 2024-07-12 18:00_

The temporary directory changes every time though -- how would you be checking if it exists?

---

_Comment by @charliermarsh on 2024-07-12 18:02_

For what it's worth, `python -m build` also fails with the same error. That suggests to me that it's a bug in the package, though not sure what. \cc @henryiii 

---

_Comment by @henryiii on 2024-07-12 18:09_

CMake-based packages should use scikit-build-core, not setuptools. :) I'd say `'-DPYTHON_INCLUDE_DIR=' + self.include_dirs[0],` is invalid, there's no guarantee in setuptools that the first include dir will be the Python include dir. `sysconfig.get_path("include")` is the correct way to get the Python include dir.

---

_Comment by @charliermarsh on 2024-07-12 18:12_

🙇  Thank you. Any idea why the build directory is present in `uv` and `build` but not `pip`? That part is a mystery to me.

---

_Comment by @henryiii on 2024-07-12 18:14_

I would have thought it would be present if any packages used the wheel "includes" folder. Maybe it's created no matter what in build/uv and only created if needed in pip? Adding `pybind11-global` is a way to get a package that using the includes wheel folder for testing. Is the folder empty?

---

_Comment by @andiradulescu on 2024-07-12 18:33_

>The temporary directory changes every time though -- how would you be checking if it exists?

In setup.py, in `def build_extension(self, ext):` I do a `os.listdir()` e.g.
```
cmake_args = [
    ...
    '-DPYTHON_INCLUDE_DIR=' + self.include_dirs[0],
    ...
]
logger.info(os.listdir(self.include_dirs[0]))
logger.info(os.listdir(os.path.dirname(self.include_dirs[0])))
```

> CMake-based packages should use scikit-build-core, not setuptools. :) I'd say `'-DPYTHON_INCLUDE_DIR=' + self.include_dirs[0],` is invalid, there's no guarantee in setuptools that the first include dir will be the Python include dir. `sysconfig.get_path("include")` is the correct way to get the Python include dir.

This sounds much better than `self.include_dirs[0]`. Thanks @henryiii! Will try this out.

>I would have thought it would be present if any packages used the wheel "includes" folder. Maybe it's created no matter what in build/uv and only created if needed in pip? Adding `pybind11-global` is a way to get a package that using the includes wheel folder for testing. Is the folder empty?

The `include` dir in the tmp folder does not exit. These are the folders in the tmp dir, at running time:
```
bin
get_requires_for_build_wheel.txt
pyvenv.cfg
CACHEDIR.TAG
.gitignore
lib
```

---

_Comment by @charliermarsh on 2024-07-12 21:17_

I think uv's behavior is correct, so gonna close.

---

_Closed by @charliermarsh on 2024-07-12 21:18_

---

_Comment by @andiradulescu on 2024-07-13 06:39_

Thanks for the help!

For me it still doesn’t make sense why there is a non-existent `include` dir there, but in `pip` isn’t, as you also said.

I would like to investigate this whole issue myself. Any pointers where this dir is injected?

As for casadi I will try to implement the correct approach suggested earlier.

Again, thanks!

---
