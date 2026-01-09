---
number: 7842
title: uv lock insists on compiling source packages
type: issue
state: closed
author: msw-kialo
labels:
  - question
assignees: []
created_at: 2024-10-01T14:00:10Z
updated_at: 2024-10-09T07:41:05Z
url: https://github.com/astral-sh/uv/issues/7842
synced_at: 2026-01-07T13:12:17-06:00
---

# uv lock insists on compiling source packages

---

_Issue opened by @msw-kialo on 2024-10-01 14:00_

I am trying to generate/refresh the `uv.lock` for a project depending on `pyicu` (native extension but no wheels published):

```toml
[project]
name = "pyicu-uv"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
  "pyicu==2.13.1"
]
```

`uv lock` fails with:

<details>
<summary>
error: Failed to download and build `pyicu==2.13.1`

  Caused by: Build backend failed to determine metadata through `prepare_metadata_for_build_wheel` with exit status: 1
</summary>

```
ubuntu@9d6444fdbf5f:/pyicu/uv$ uv lock --no-build-isolation
Using CPython 3.12.6 interpreter at: /opt/containerbase/tools/python/3.12.6/bin/python
⠙ pyicu==2.13.1                                                                                                                                                                                                                                            error: Failed to download and build `pyicu==2.13.1`
  Caused by: Build backend failed to determine metadata through `prepare_metadata_for_build_wheel` with exit status: 1
--- stdout:
(running 'icu-config --version')
(running 'pkg-config --modversion icu-i18n')
--- stderr:
Package icu-i18n was not found in the pkg-config search path.
Perhaps you should add the directory containing `icu-i18n.pc'
to the PKG_CONFIG_PATH environment variable
No package 'icu-i18n' found
Traceback (most recent call last):
  File "<string>", line 89, in <module>
  File "<frozen os>", line 714, in __getitem__
KeyError: 'ICU_VERSION'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<string>", line 92, in <module>
  File "<string>", line 19, in check_output
  File "/opt/containerbase/tools/python/3.12.6/lib/python3.12/subprocess.py", line 466, in check_output
    return run(*popenargs, stdout=PIPE, timeout=timeout, check=True,
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/containerbase/tools/python/3.12.6/lib/python3.12/subprocess.py", line 548, in run
    with Popen(*popenargs, **kwargs) as process:
         ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/containerbase/tools/python/3.12.6/lib/python3.12/subprocess.py", line 1026, in __init__
    self._execute_child(args, executable, preexec_fn, close_fds,
  File "/opt/containerbase/tools/python/3.12.6/lib/python3.12/subprocess.py", line 1955, in _execute_child
    raise child_exception_type(errno_num, err_msg, err_filename)
FileNotFoundError: [Errno 2] No such file or directory: 'icu-config'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<string>", line 96, in <module>
  File "<string>", line 19, in check_output
  File "/opt/containerbase/tools/python/3.12.6/lib/python3.12/subprocess.py", line 466, in check_output
    return run(*popenargs, stdout=PIPE, timeout=timeout, check=True,
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/containerbase/tools/python/3.12.6/lib/python3.12/subprocess.py", line 571, in run
    raise CalledProcessError(retcode, process.args,
subprocess.CalledProcessError: Command '('pkg-config', '--modversion', 'icu-i18n')' returned non-zero exit status 1.

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/opt/containerbase/tools/python/3.12.6/lib/python3.12/site-packages/setuptools/build_meta.py", line 373, in prepare_metadata_for_build_wheel
    self.run_setup()
  File "/opt/containerbase/tools/python/3.12.6/lib/python3.12/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 99, in <module>
RuntimeError: 
Please install pkg-config on your system or set the ICU_VERSION environment
variable to the version of ICU you have installed.
        
---
```

</details>

(alternative `uv lock --no-build` with "Building source distributions is disabled").

I get that building the source distribution is required to *install* the package, but I just want to generate/update the lock file to be used on other systems.


<details>
<summary>I bring this up as `poetry` (what we try to replace with `uv`), is able to lock a corresponding project (and the resulting lockfile contains the same content). </summary>

`pyproject.toml`:

```toml
[tool.poetry]
name = "pyicu-poetry"
version = "0.1.0"
description = ""
authors = ["Your Name <you@example.com>"]
readme = "README.md"

[tool.poetry.dependencies]
python = "^3.12"
PyICU = "^2.13.1"


[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

```
ubuntu@9d6444fdbf5f:/pyicu/poetry$ poetry lock
Creating virtualenv pyicu-poetry-g8m-gjSW-py3.12 in /home/ubuntu/.cache/pypoetry/virtualenvs
Updating dependencies
Resolving dependencies... (7.0s)

Writing lock file
```

Generates

```toml
# This file is automatically @generated by Poetry 1.8.3 and should not be changed by hand.

[[package]]
name = "pyicu"
version = "2.13.1"
description = "Python extension wrapping the ICU C++ API"
optional = false
python-versions = "*"
files = [
    {file = "PyICU-2.13.1.tar.gz", hash = "sha256:d4919085eaa07da12bade8ee721e7bbf7ade0151ca0f82946a26c8f4b98cdceb"},
]

[metadata]
lock-version = "2.0"
python-versions = "^3.12"
content-hash = "ffe7de881276f533ee12cbc50bc380962d85900ee21c2c80380bd45e474915ce"
```

uv's lock-file (on systems with ICU & co) does contain roughly the same information:

```
version = 1
requires-python = ">=3.12"

[[package]]
name = "pyicu"
version = "2.13.1"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/60/b8/1540a0a0cd74aa878749d442e19916df946e3b187c9965a991ddc77cc39c/PyICU-2.13.1.tar.gz", hash = "sha256:d4919085eaa07da12bade8ee721e7bbf7ade0151ca0f82946a26c8f4b98cdceb", size = 262424 }

[[package]]
name = "pyicu-uv"
version = "0.1.0"
source = { virtual = "." }
dependencies = [
    { name = "pyicu" },
]

[package.metadata]
requires-dist = [{ name = "pyicu", specifier = "==2.13.1" }]
```

</details>

Could it be that `uv` does not use an already existing `PKG-INFO` but always invokes `setup.py`?

I am using `0.4.17` in an amd64 docker container.

Background: I try to setup renovate updates for our uv project, but renovate fails to run `uv lock` as ICU isn't installed.

---

_Comment by @zanieb on 2024-10-01 14:23_

I believe this project does not declare static metadata so we need to build the wheel in order to know its dependencies. The [`PKG-INFO`](https://inspector.pypi.io/project/pyicu/2.13.1/packages/60/b8/1540a0a0cd74aa878749d442e19916df946e3b187c9965a991ddc77cc39c/PyICU-2.13.1.tar.gz/pyicu-2.13.1/PKG-INFO) does not include dependency metadata.

I presume Poetry is just assuming the package has no dependencies here.

---

_Label `question` added by @zanieb on 2024-10-01 14:23_

---

_Comment by @msw-kialo on 2024-10-01 16:09_

Thank you for clarification. I reread the [spec](https://packaging.python.org/en/latest/specifications/core-metadata/#core-metadata) to learn that unless `Dynamic` is specific in metadata 2.2 and higher, all fields must be considered dynamic.

In that case, resolve this issues differently (either by making ICU development headers available or raise an issue with PyICU).

---

_Closed by @msw-kialo on 2024-10-01 16:09_

---

_Comment by @dimbleby on 2024-10-02 07:34_

The linked pkg-info is using metadata version 2.1, so `Dynamic` is not in play and poetry trusts it when it declares no requirements.

---

_Comment by @msw-kialo on 2024-10-02 07:56_

Yes, the `Dynamic` field isn't allowed yet; but for tools that do know it, should assume it is specific with all fields:

> If the sdist metadata version is older than version 2.2, then all fields should be treated as if they were specified with Dynamic (i.e. there are no special restrictions on the metadata of wheels built from the sdist).

---

_Comment by @dimbleby on 2024-10-02 10:58_

you're right, I had misremembered the way this works

the actual reason that poetry doesn't perform a build during locking seems to be that it asks the `build` project for the metadata path, and that successfully calls `prepare_metadata_for_build_wheel` without performing a full build, and everyone is happy with that.

not sure whether that's right or not...

---

_Comment by @msw-kialo on 2024-10-07 07:57_

@zanieb Would it be possible to use `prepare_metadata_for_build_wheel` in `uv`, too? Or does it not cover all edge cases?

---

_Reopened by @msw-kialo on 2024-10-07 07:57_

---

_Comment by @charliermarsh on 2024-10-08 22:29_

We always use `prepare_metadata_for_build_wheel`, yeah. You can see it in the trace above.

---

_Comment by @msw-kialo on 2024-10-09 07:41_

Sorry, my bad that I didn't verify that.

---

_Closed by @msw-kialo on 2024-10-09 07:41_

---
