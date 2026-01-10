---
number: 6052
title: "Can't install wheel in working directory without specifying `./`"
type: issue
state: closed
author: ilkersigirci
labels:
  - question
  - error messages
assignees: []
created_at: 2024-08-13T00:26:11Z
updated_at: 2024-09-09T11:49:28Z
url: https://github.com/astral-sh/uv/issues/6052
synced_at: 2026-01-10T01:23:55Z
---

# Can't install wheel in working directory without specifying `./`

---

_Issue opened by @ilkersigirci on 2024-08-13 00:26_

- I want to use  `uv` inside docker to install wheel packages. Unfortunately it doesn't work and gives following error
```
No solution found when resolving dependencies:
  ╰─▶ Because python-dotenv-1-0-1-py3-none-any-whl was not found in the package registry and you require
    python-dotenv-1-0-1-py3-none-any-whl, we can conclude that the requirements are unsatisfiable. 
```
- I have used python-dotenv wheel as an example. It doesn't work on any wheel files.
- BTW, this behaviour is only inside Docker, outside the Docker, everything works as expected.


- Example Dockerfile
```yaml
ARG PYTHON_VERSION=3.11
FROM python:${PYTHON_VERSION}-slim-bookworm as production

ENV \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PYTHONFAULTHANDLER=1 \
    PIP_ROOT_USER_ACTION=ignore \
    UV_SYSTEM_PYTHON=1 \
    UV_NO_CONFIG=1 \
    UV_NO_CACHE=1

WORKDIR /app

RUN \
    apt-get update -y \
    && apt-get install -y --no-install-recommends \
    curl \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /var/cache/apt/archives

ADD --chmod=755 https://astral.sh/uv/install.sh /install.sh
RUN /install.sh && rm /install.sh

ENV PATH="/root/.cargo/bin/:$PATH"

RUN curl \
    https://files.pythonhosted.org/packages/6a/3e/b68c118422ec867fa7ab88444e1274aa40681c606d59ac27de5a5588f082/python_dotenv-1.0.1-py3-none-any.whl \
    --output python_dotenv-1.0.1-py3-none-any.whl


# NOTE: Gives error
RUN uv pip install python_dotenv-1.0.1-py3-none-any.whl
```

---

_Comment by @zanieb on 2024-08-13 00:47_

Hi! I think you just need to disambiguate that it's a file path and not a package name, e.g. with `./python_dotenv-1.0.1-py3-none-any.whl`.

@charliermarsh I wonder if we should detect this automatically for names ending in `.whl`?

---

_Label `question` added by @zanieb on 2024-08-13 00:47_

---

_Label `error messages` added by @zanieb on 2024-08-13 00:47_

---

_Comment by @zanieb on 2024-08-13 00:48_

I think we should also add a hint to resolver errors when the package is not found in the registry but the name exists in the working directory.

---

_Comment by @ilkersigirci on 2024-08-13 06:34_

> Hi! I think you just need to disambiguate that it's a file path and not a package name, e.g. with `./python_dotenv-1.0.1-py3-none-any.whl`.
> 
> @charliermarsh I wonder if we should detect this automatically for names ending in `.whl`?

Thank you, this resolves the issue. I agree with you on the error message part. It would be nice if the error message can be more clear 

---

_Closed by @ilkersigirci on 2024-08-13 06:34_

---

_Reopened by @zanieb on 2024-08-13 19:00_

---

_Comment by @zanieb on 2024-08-13 19:00_

I'll leave this open until we know what we want to do for improvements.

---

_Renamed from "Can't install wheel inside Docker" to "Can't install wheel in working directory without specifying `./`" by @zanieb on 2024-08-13 19:00_

---

_Referenced in [python-trio/trio#2957](../../python-trio/trio/pulls/2957.md) on 2024-08-14 22:51_

---

_Comment by @lexiconlp on 2024-09-04 16:54_

> BTW, this behaviour is only inside Docker, outside the Docker, everything works as expected.


I test in a Mac ( not in docker ) m1 and this fail ( I would like to know why and how to solve it )


```shell
$ uv --version
$ uv init test-project --python 3.12
$ cd test-project 
$ uv add dotenv
💥 💥 
```
All details here: 

```shell
$ uv --version
uv 0.4.3 (47f4ca24b 2024-09-02)
$ uv init test-project --python 3.12
Initialized project `test-project` at `/Users/my-user/Documents/test-project`
$ cd test-project 
$ uv add dotenv
Using Python 3.12.0 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
Creating virtualenv at: .venv
⠸ dotenv==0.0.5                                                                 error: Failed to download and build `dotenv==0.0.5`
  Caused by: Failed to install requirements from `build-system.requires` (resolve)
  Caused by: No solution found when resolving: setuptools>=40.8.0, distribute
  Caused by: Failed to download and build `distribute==0.6.49`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/Users/my-user/.cache/uv/builds-v0/.tmpzAmVk8/lib/python3.12/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/my-user.cache/uv/builds-v0/.tmpzAmVk8/lib/python3.12/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
    self.run_setup()
  File "/Users/my-user/.cache/uv/builds-v0/.tmpzAmVk8/lib/python3.12/site-packages/setuptools/build_meta.py", line 503, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/Users/my-user/.cache/uv/builds-v0/.tmpzAmVk8/lib/python3.12/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 34, in <module>
AttributeError: module 'distutils.util' has no attribute 'run_2to3'
---

```

---

_Comment by @zanieb on 2024-09-04 16:59_

@lexiconlp I think you're looking for `uv add python-dotenv`.

---

_Referenced in [astral-sh/uv#7035](../../astral-sh/uv/issues/7035.md) on 2024-09-04 17:00_

---

_Comment by @nibbledeez on 2024-09-06 15:35_

@zanieb - I also get the same issue on Ubutnu 22.04 with python-dotenv:

❯ uv add python-dotenv
⠹ distribute==0.6.49                                                                                                                                                                                                                        error: Failed to download and build `distribute==0.6.49`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/home/my-user/.cache/uv/builds-v0/.tmpLivn31/lib/python3.12/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/my-user/.cache/uv/builds-v0/.tmpLivn31/lib/python3.12/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
    self.run_setup()
  File "/home/my-user/.cache/uv/builds-v0/.tmpLivn31/lib/python3.12/site-packages/setuptools/build_meta.py", line 503, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/home/my-user/.cache/uv/builds-v0/.tmpLivn31/lib/python3.12/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 34, in <module>
AttributeError: module 'distutils.util' has no attribute 'run_2to3'


---

_Comment by @charliermarsh on 2024-09-06 15:36_

That looks like a legitimate error with the package itself?

---

_Comment by @nibbledeez on 2024-09-06 15:40_

Perhaps the issue is 'dependency' as I get the same with bs4 as below. Both bs4 and python-dotenv install fine using pip and the same env.

❯ uv add bs4
⠹ distribute==0.6.49                                                                                                                                                                                                                        error: Failed to download and build `distribute==0.6.49`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/home/my-user/.cache/uv/builds-v0/.tmpI7wM21/lib/python3.12/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/my-user/.cache/uv/builds-v0/.tmpI7wM21/lib/python3.12/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
    self.run_setup()
  File "/home/my-user/.cache/uv/builds-v0/.tmpI7wM21/lib/python3.12/site-packages/setuptools/build_meta.py", line 503, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/home/my-user/.cache/uv/builds-v0/.tmpI7wM21/lib/python3.12/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 34, in <module>
AttributeError: module 'distutils.util' has no attribute 'run_2to3'

---

_Comment by @notatallshaw on 2024-09-06 15:49_

Yeah, I wouldn't expect `distribute` to work with modern package requirements, it's very old: https://pypi.org/project/distribute/#history

Where is distribute coming from? It shouldn't be a dependency of any modern package. It's project description is "Distribute - legacy package. This package is a simple compatibility layer that installs Setuptools 0.7+."


---

_Comment by @charliermarsh on 2024-09-06 15:50_

If you include `--verbose` output`, we can probably figure out why it's even being requested. You might be backtracking very far back due to some constraint.

---

_Comment by @nibbledeez on 2024-09-07 13:01_

Ok - here it is with `--verbose` - I've created a new empty project as there was a lot of stuff about previously installed packages. (Just as an aside this is not critical but really excited about uv - thank you!)

```
❯ uv add --verbose dotenv  
DEBUG uv 0.4.6
DEBUG Found project root: `/home/my-user/tmp/uvtest`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `/home/my-user/tmp/uvtest/.python-version`
DEBUG The virtual environment's Python version satisfies `Python 3.12`
DEBUG Using request timeout of 30s
DEBUG Using request timeout of 30s
DEBUG Starting clean resolution
DEBUG Acquired lock for `/home/my-user/.cache/uv/built-wheels-v3/path/ae978f7ca6814a32`
DEBUG Found static `pyproject.toml` for: uvtest @ file:///home/my-user/tmp/uvtest
DEBUG No workspace root found, using project root
DEBUG Released lock at `/home/my-user/.cache/uv/built-wheels-v3/path/ae978f7ca6814a32/.lock`
DEBUG Solving with installed Python version: 3.12.5
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: uvtest*
DEBUG Searching for a compatible version of uvtest @ file:///home/my-user/tmp/uvtest (*)
DEBUG Adding transitive dependency for uvtest==0.1.0: dotenv*
DEBUG Found fresh response for: https://pypi.org/simple/dotenv/
DEBUG Searching for a compatible version of dotenv (*)
DEBUG Selecting: dotenv==0.0.5 [compatible] (dotenv-0.0.5.tar.gz)
DEBUG Acquired lock for `/home/my-user/.cache/uv/built-wheels-v3/pypi/dotenv/0.0.5`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e2/46/3754073706e31670eed18bfa8a879305b56a471db15f20523c2427b10078/dotenv-0.0.5.tar.gz
DEBUG No static `pyproject.toml` available for: dotenv==0.0.5 (MissingPyprojectToml)
DEBUG No static `PKG-INFO` available for: dotenv==0.0.5 (PkgInfo(UnsupportedMetadataVersion("1.0")))
DEBUG No static `egg-info` available for: dotenv==0.0.5 (MissingRequiresTxt)
DEBUG Preparing metadata for: dotenv==0.0.5
DEBUG Ignoring empty directory
DEBUG Solving with installed Python version: 3.12.5
DEBUG Solving with target Python version: >=3.12.5
DEBUG Adding direct dependency: setuptools>=40.8.0
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
DEBUG Selecting: setuptools==74.1.2 [compatible] (setuptools-74.1.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cb/9c/9ad11ac06b97e55ada655f8a6bea9d1d3f06e120b178cd578d80e558191d/setuptools-74.1.2-py3-none-any.whl.metadata
DEBUG Tried 1 versions: setuptools 1
DEBUG Split specific environment resolution took 0.001s
DEBUG Installing in setuptools==74.1.2 in /home/my-user/.cache/uv/builds-v0/.tmp331A8R
DEBUG Requirement already cached: setuptools==74.1.2
DEBUG Installing build requirement: setuptools==74.1.2
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
DEBUG Installing extra requirements for build backend
DEBUG Solving with installed Python version: 3.12.5
DEBUG Solving with target Python version: >=3.12.5
DEBUG Adding direct dependency: setuptools>=40.8.0
DEBUG Adding direct dependency: distribute*
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
DEBUG Selecting: setuptools==74.1.2 [compatible] (setuptools-74.1.2-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/distribute/
DEBUG Searching for a compatible version of distribute (*)
DEBUG Selecting: distribute==0.7.3 [compatible] (distribute-0.7.3.zip)
DEBUG Acquired lock for `/home/my-user/.cache/uv/built-wheels-v3/pypi/distribute/0.7.3`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5f/ad/1fde06877a8d7d5c9b60eff7de2d452f639916ae1d48f0b8f97bf97e570a/distribute-0.7.3.zip
DEBUG No static `pyproject.toml` available for: distribute==0.7.3 (MissingPyprojectToml)
DEBUG No static `PKG-INFO` available for: distribute==0.7.3 (PkgInfo(UnsupportedMetadataVersion("1.1")))
DEBUG Found static `egg-info` for: distribute==0.7.3
DEBUG Released lock at `/home/my-user/.cache/uv/built-wheels-v3/pypi/distribute/0.7.3/.lock`
WARN Unable to extract metadata for distribute: Package metadata name `setuptools` does not match given name `distribute`
DEBUG Searching for a compatible version of distribute (<0.7.3 | >0.7.3)
DEBUG Selecting: distribute==0.6.49 [compatible] (distribute-0.6.49.tar.gz)
DEBUG Acquired lock for `/home/my-user/.cache/uv/built-wheels-v3/pypi/distribute/0.6.49`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f3/2b/e97c01487b7636ba3fcff5e73db995c66c524d54d0907d238311524f67c8/distribute-0.6.49.tar.gz
DEBUG No static `pyproject.toml` available for: distribute==0.6.49 (MissingPyprojectToml)
DEBUG No static `PKG-INFO` available for: distribute==0.6.49 (PkgInfo(UnsupportedMetadataVersion("1.1")))
DEBUG No static `egg-info` available for: distribute==0.6.49 (MissingRequiresTxt)
DEBUG Preparing metadata for: distribute==0.6.49
DEBUG Ignoring empty directory
DEBUG Installing in setuptools==74.1.2 in /home/my-user/.cache/uv/builds-v0/.tmpUjuoro
DEBUG Requirement already cached: setuptools==74.1.2
DEBUG Installing build requirement: setuptools==74.1.2
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
DEBUG Traceback (most recent call last):
DEBUG 

DEBUG   File "<string>", line 14, in <module>
DEBUG 

DEBUG   File "/home/my-user/.cache/uv/builds-v0/.tmpUjuoro/lib/python3.12/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
DEBUG 

DEBUG     return self._get_build_requires(config_settings, requirements=[])
DEBUG 

DEBUG            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
DEBUG 

DEBUG   File "/home/my-user/.cache/uv/builds-v0/.tmpUjuoro/lib/python3.12/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
DEBUG 

DEBUG     self.run_setup()
DEBUG 

DEBUG   File "/home/my-user/.cache/uv/builds-v0/.tmpUjuoro/lib/python3.12/site-packages/setuptools/build_meta.py", line 503, in run_setup
DEBUG 

DEBUG     super().run_setup(setup_script=setup_script)
DEBUG 

DEBUG   File "/home/my-user/.cache/uv/builds-v0/.tmpUjuoro/lib/python3.12/site-packages/setuptools/build_meta.py", line 318, in run_setup
DEBUG 

DEBUG     exec(code, locals())
DEBUG 

DEBUG   File "<string>", line 34, in <module>
DEBUG 

DEBUG AttributeError: module 'distutils.util' has no attribute 'run_2to3'
DEBUG 

DEBUG Released lock at `/home/my-user/.cache/uv/built-wheels-v3/pypi/distribute/0.6.49/.lock`
DEBUG Released lock at `/home/my-user/.cache/uv/built-wheels-v3/pypi/dotenv/0.0.5/.lock`
error: Failed to download and build `dotenv==0.0.5`
  Caused by: Failed to install requirements from `build-system.requires` (resolve)
  Caused by: No solution found when resolving: setuptools>=40.8.0, distribute
  Caused by: Failed to download and build `distribute==0.6.49`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/home/my-user/.cache/uv/builds-v0/.tmpUjuoro/lib/python3.12/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/my-user/.cache/uv/builds-v0/.tmpUjuoro/lib/python3.12/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
    self.run_setup()
  File "/home/my-user/.cache/uv/builds-v0/.tmpUjuoro/lib/python3.12/site-packages/setuptools/build_meta.py", line 503, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/home/my-user/.cache/uv/builds-v0/.tmpUjuoro/lib/python3.12/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 34, in <module>
AttributeError: module 'distutils.util' has no attribute 'run_2to3'


```

---

_Comment by @nibbledeez on 2024-09-07 13:31_

> Yeah, I wouldn't expect `distribute` to work with modern package requirements, it's very old: https://pypi.org/project/distribute/#history
> 
> Where is distribute coming from? It shouldn't be a dependency of any modern package. It's project description is "Distribute - legacy package. This package is a simple compatibility layer that installs Setuptools 0.7+."

Hmm - not sure but perhaps this is germane?

```
error: Failed to download and build `dotenv==0.0.5`
  Caused by: Failed to install requirements from `build-system.requires` (resolve)
  Caused by: **No solution found when resolving: setuptools>=40.8.0, distribute**
  Caused by: Failed to download and build `distribute==0.6.49`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
```

---

_Comment by @charliermarsh on 2024-09-07 14:46_

Apologies, do you mean to be installing `dotenv==0.0.5`? You might be intending to do `uv add python-dotenv` instead.

---

_Comment by @nibbledeez on 2024-09-09 07:45_

Ok - so I had tried python-dotenv as well as thought perhaps I had the wrong package - that also failed. However,  manually deleting the dotenv dir and files from .venv from the first attempt has worked. Guess this one can be closed.

---

_Closed by @charliermarsh on 2024-09-09 11:49_

---
