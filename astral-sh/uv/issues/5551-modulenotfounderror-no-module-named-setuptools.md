```yaml
number: 5551
title: "ModuleNotFoundError: No module named 'setuptools.command.test'"
type: issue
state: closed
author: yu-iskw
labels:
  - external
assignees: []
created_at: 2024-07-29T05:13:04Z
updated_at: 2024-07-29T14:24:52Z
url: https://github.com/astral-sh/uv/issues/5551
synced_at: 2026-01-12T15:58:56Z
```

# ModuleNotFoundError: No module named 'setuptools.command.test'

---

_@yu-iskw_

## Overview

I got the similar error `ModuleNotFoundError: No module named 'setuptools.command.test'` with the install command on GitHub Actions as the benchmarks workflow was failed.

```
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: logbook==1.5.3
  Caused by: Failed to build: `logbook==1.5.3`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:
--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/home/runner/.cache/uv/builds-v0/.tmp6kYDCi/lib/python3.11/site-packages/setuptools/build_meta.py", line 327, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/.cache/uv/builds-v0/.tmp6kYDCi/lib/python3.11/site-packages/setuptools/build_meta.py", line 297, in _get_build_requires
    self.run_setup()
  File "/home/runner/.cache/uv/builds-v0/.tmp6kYDCi/lib/python3.11/site-packages/setuptools/build_meta.py", line 497, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/home/runner/.cache/uv/builds-v0/.tmp6kYDCi/lib/python3.11/site-packages/setuptools/build_meta.py", line 313, in run_setup
    exec(code, locals())
  File "<string>", line 64, in <module>
ModuleNotFoundError: No module named 'setuptools.command.test'
```

## The failed benchmarks workflow
https://github.com/astral-sh/uv/actions/runs/10137169832/job/28027028298
```
error: Failed to download and build `pyhive==0.7.0`
  Caused by: Failed to build: `pyhive==0.7.0`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/home/runner/work/uv/uv/.cache/builds-v0/.tmpuS9Fnv/lib/python3.10/site-packages/setuptools/build_meta.py", line 327, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
  File "/home/runner/work/uv/uv/.cache/builds-v0/.tmpuS9Fnv/lib/python3.10/site-packages/setuptools/build_meta.py", line 297, in _get_build_requires
    self.run_setup()
  File "/home/runner/work/uv/uv/.cache/builds-v0/.tmpuS9Fnv/lib/python3.10/site-packages/setuptools/build_meta.py", line 497, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/home/runner/work/uv/uv/.cache/builds-v0/.tmpuS9Fnv/lib/python3.10/site-packages/setuptools/build_meta.py", line 313, in run_setup
    exec(code, locals())
  File "<string>", line 4, in <module>
ModuleNotFoundError: No module named 'setuptools.command.test'
```

---

_Comment by @bardiharborow on 2024-07-29 05:34_

setuptools 72.0.0 (released 3 hours ago) has caused certain Python packages to break (pypa/setuptools#4519). This is compounded by [uv's installation of the latest version of setuptools](https://github.com/astral-sh/uv/blob/51b7e9bff1cbf0c8ad18c5447f808176e902db22/crates/uv/src/commands/venv.rs#L278) during venv setup.

---

_Comment by @hauntsaninja on 2024-07-29 05:38_

This isn't uv's fault. See https://github.com/pypa/setuptools/issues/4519#issuecomment-2254983472 , but use `UV_CONSTRAINT` env var instead

Edit: oops, `UV_CONSTRAINT` doesn't actually work the same way, see https://github.com/astral-sh/uv/issues/5551#issuecomment-2255039974

---

_Comment by @yu-iskw on 2024-07-29 05:47_

@hauntsaninja Thank you for the help. That works! Let me close the issue. 

---

_Closed by @yu-iskw on 2024-07-29 05:47_

---

_Comment by @hauntsaninja on 2024-07-29 05:53_

I'd still be interested if any uv folks have ideas for how to improve reproducibility of isolated build envs.

Here's one (probably dumb) suggestion. `uv pip install -c constraints.txt ...` should apply the constraints to isolated build environments as well. Even if not dumb, maybe too much of a departure from pip though...

---

_Comment by @delfick on 2024-07-29 06:07_

it seems that UV_CONSTRAINT or `--constraint` flag isn't working to restrict the version that uv uses

Using this as setuptools-constraint.txt
```
setuptools==70.3.0
```

Seems uv still uses the latest setuptools


```
╰─ ./tools/uv pip install envparse --constraint setuptools-constraint.txt -v
Audited 1 package in 55ms
DEBUG uv 0.2.29
DEBUG Searching for Python interpreter in system path
DEBUG Found `cpython-3.10.14-macos-aarch64-none` at `/Users/stephen.moore/.virtualenvs/kraken4/bin/python3` (active virtual environment)
DEBUG Using Python 3.10.14 environment at /Users/stephen.moore/.virtualenvs/kraken4/bin/python3
DEBUG Acquired lock for `/Users/stephen.moore/.virtualenvs/kraken4`
DEBUG At least one requirement is not satisfied: envparse
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.10.14
DEBUG Adding direct dependency: envparse*
DEBUG No cache entry for: https://pypi.org/simple/envparse/
DEBUG Searching for a compatible version of envparse (*)
DEBUG Selecting: envparse==0.2.0 [compatible] (envparse-0.2.0.tar.gz)
DEBUG Acquired lock for `/Users/stephen.moore/Library/Caches/uv/built-wheels-v3/pypi/envparse/0.2.0`
DEBUG No cache entry for: https://files.pythonhosted.org/packages/2f/8d/bee8a59732c169a455627ff1557d0db180f7c352b0274480267ad3e46875/envparse-0.2.0.tar.gz
DEBUG Downloading source distribution: envparse==0.2.0
DEBUG Preparing metadata for: envparse==0.2.0
DEBUG No static `PKG-INFO` available for: envparse==0.2.0 (PkgInfo(UnsupportedMetadataVersion("1.1")))
DEBUG No static `pyproject.toml` available for: envparse==0.2.0 (MissingPyprojectToml)
INFO Ignoring empty directory
DEBUG Solving with installed Python version: 3.10.14
DEBUG Adding direct dependency: setuptools>=40.8.0
DEBUG No cache entry for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
DEBUG Selecting: setuptools==72.0.0 [compatible] (setuptools-72.0.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/2f/83/06edd551b4fdf6170dcbafeeed588a8909819e943905c182ebdc98835be8/setuptools-72.0.0-py3-none-any.whl.metadata
DEBUG Tried 1 versions: setuptools 1
DEBUG Split specific environment resolution took 0.181s
DEBUG Installing in setuptools==72.0.0 in /Users/stephen.moore/Library/Caches/uv/builds-v0/.tmpn7RJ3U
DEBUG Identified uncached requirement: setuptools==72.0.0
DEBUG Downloading and building requirement for build: setuptools==72.0.0
DEBUG No cache entry for: https://files.pythonhosted.org/packages/2f/83/06edd551b4fdf6170dcbafeeed588a8909819e943905c182ebdc98835be8/setuptools-72.0.0-py3-none-any.whl
DEBUG Installing build requirement: setuptools==72.0.0
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
error: Failed to download and build `envparse==0.2.0`
  Caused by: Failed to build: `envparse==0.2.0`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/Users/stephen.moore/Library/Caches/uv/builds-v0/.tmpn7RJ3U/lib/python3.10/site-packages/setuptools/build_meta.py", line 327, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
  File "/Users/stephen.moore/Library/Caches/uv/builds-v0/.tmpn7RJ3U/lib/python3.10/site-packages/setuptools/build_meta.py", line 297, in _get_build_requires
    self.run_setup()
  File "/Users/stephen.moore/Library/Caches/uv/builds-v0/.tmpn7RJ3U/lib/python3.10/site-packages/setuptools/build_meta.py", line 497, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/Users/stephen.moore/Library/Caches/uv/builds-v0/.tmpn7RJ3U/lib/python3.10/site-packages/setuptools/build_meta.py", line 313, in run_setup
    exec(code, locals())
  File "<string>", line 4, in <module>
ModuleNotFoundError: No module named 'setuptools.command.test'
---


```

---

_Comment by @hauntsaninja on 2024-07-29 06:24_

Oops, so it is:

```
λ cat c.in
setuptools<70

λ PIP_CONSTRAINT=c.in pip install moviepy --no-cache-dir
...
Installing collected packages: moviepy
...
Successfully installed moviepy-1.0.3

λ uv pip uninstall moviepy --system                     
Uninstalled 1 package in 21ms
 - moviepy==1.0.3

λ UV_CONSTRAINT=c.in PIP_CONSTRAINT=c.in uv pip install moviepy --system
⠹ moviepy==1.0.3                                                                                                                                                            error: Failed to download and build `moviepy==1.0.3`
  Caused by: Failed to build: `moviepy==1.0.3`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/root/.cache/uv/builds-v0/.tmp06f83o/lib/python3.11/site-packages/setuptools/build_meta.py", line 327, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/root/.cache/uv/builds-v0/.tmp06f83o/lib/python3.11/site-packages/setuptools/build_meta.py", line 297, in _get_build_requires
    self.run_setup()
  File "/root/.cache/uv/builds-v0/.tmp06f83o/lib/python3.11/site-packages/setuptools/build_meta.py", line 497, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/root/.cache/uv/builds-v0/.tmp06f83o/lib/python3.11/site-packages/setuptools/build_meta.py", line 313, in run_setup
    exec(code, locals())
  File "<string>", line 21, in <module>
NameError: name 'TestCommand' is not defined
---
```
(same deal with e.g. `vcrpy`)

@yu-iskw could you re-open?

---

_Reopened by @yu-iskw on 2024-07-29 06:29_

---

_Comment by @yu-iskw on 2024-07-29 06:35_

@hauntsaninja @delfick Indeed I didn't solve the issue only with `UV_CONSTRAINTS` and `--no-build-isolation`. In my case, I needed to add the dependency on `wheel` as below. I mainly use `uv` in GitHub Actions. As I install dependencies to system with `uv` in GitHub Action, I have the same requirements file to set up the basics to use `uv` and run a `uv pip install` command.

1. Run `python -m pip install --no-cache -r requirements-setup.txt`
2. Run `python -m uv pip install ...` with `UV_CONSTRAINTS=path/to/requirements-setup.txt`. 

```
# requirements-setup.txt
pip==24.2
setuptools==71.0.0
wheel>=0.40,<1.0
uv>=0.2,<0.3
```

---

_Comment by @delfick on 2024-07-29 06:48_

if you do a `uv cache clean` and do it again, does it still work?

Cause adding those extra things to the constraints doesn't make a difference for me

---

_Comment by @weihenglim on 2024-07-29 07:14_

Using the `--no-build-isolation` flag works for me (`uv` correctly uses the installed version of `setuptools`)

---

_Comment by @MatthijsKok on 2024-07-29 09:11_

> Using the `--no-build-isolation` flag works for me (`uv` correctly uses the installed version of `setuptools`)

Can confirm, this fixed the issue for us as well.

---

_Comment by @delfick on 2024-07-29 09:13_

(the `--no-build-isolation` works btw because it uses the setuptools in your environment, rather than creating an isolated environment for building, and should probably be considered a temporary fix)

---

_Comment by @konstin on 2024-07-29 09:15_

> I'd still be interested if any uv folks have ideas for how to improve reproducibility of isolated build envs.

We should lock build environments, see https://github.com/astral-sh/uv/issues/5190

---

_Comment by @ghost on 2024-07-29 09:44_

> Using the `--no-build-isolation` flag works for me (`uv` correctly uses the installed version of `setuptools`)

Can also confirm that this works as a temporary workaround, after pinning `setuptools<72.0.0`.

---

_Comment by @yu-iskw on 2024-07-29 10:15_

How about add the version constraint bot to use setuptools 72.0.0 until the issue is resolved? It might be hassle for each uv user to apply the workaround to its environment.

---

_Comment by @ghost on 2024-07-29 10:18_

@yu-iskw It would need to be <72.0.0, right? As that is the version that introduces the breaking changes.

---

_Comment by @yu-iskw on 2024-07-29 10:27_

@josephd-envsys that's right. I totally understand the issue should be only on the setuptools side. And I'm not sure how the setuptools community handle it. But the impact of the issue is quite broad. My proposal should be a tentative solution in uv. But implementing the tentative workaround in uv would be useful for many uv users.

---

_Comment by @ghost on 2024-07-29 10:29_

@yu-iskw quite.

Have a look at the issue over at setuptools: https://github.com/pypa/setuptools/issues/4519

**TLDR:**
> - This functionality has been deprecated for 5 years, there is a separate issue for discussing if there would have been a better way of removing the functionality https://github.com/pypa/setuptools/issues/4520
> - For people using pip (or tools using pip, like poetry), PIP_CONSTRAINT set to a file with setuptools<72.0 should work
> - For people using uv, there seems to be a bug where UV_CONSTRAINT doesn't affect the version of setuptools used in build isolation (see https://github.com/astral-sh/uv/issues/5551) and in that case the solution is either --no-build-isolation (which should be considered a temporary solution) or getting packages to not use setuptools.command.test or using forks of those packages that comment out the use of setuptools.command.test (until setuptools releases a fix/revert)

---

_Comment by @alejobs on 2024-07-29 10:39_

Hi,

I'm not able to find a workaround for this as I'm using poetry. There is not option like "--no-build-isolation" in poetry. Can anyone help me please?

Thanks.

---

_Comment by @ghost on 2024-07-29 10:44_

@alejobs give this workaround a go: https://github.com/pypa/setuptools/issues/4519#issuecomment-2255381827


> ```dockerfile
> WORKDIR /code
> # these two new lines are the fix
> RUN echo "setuptools<72" > "constraints.txt"
> ENV PIP_CONSTRAINT=/code/constraints.txt
>
> RUN poetry install --no-interaction --no-ansi
> ```

---

_Label `upstream` added by @konstin on 2024-07-29 11:37_

---

_Comment by @charliermarsh on 2024-07-29 12:37_

So, does pip apply constraints to build environments? It’s easy for us to do that but I think it’s intentional that we don’t (at least right now).

---

_Comment by @tomwojcik on 2024-07-29 13:32_

setuptools 72.0.0 is yanked

https://github.com/pypa/setuptools/issues/4519#issuecomment-2255940870

---

_Comment by @charliermarsh on 2024-07-29 13:37_

For the future, you can also set `UV_EXCLUDE_NEWER=2024-07-27` or similar to avoid pulling in new versions, at least temporarily.

---

_Comment by @yu-iskw on 2024-07-29 13:41_

As setuptools 72.0.0 has been yanked, I suppose we no longer need the workaround at the moment. I will close the issue in a few days for the visibility just in case, because some users might be still confused by the deprecation issue. Thank you all for your help.

https://pypi.org/project/setuptools/#history

---

_Comment by @charliermarsh on 2024-07-29 13:42_

Thanks. Still wondering if we should apply constraints to build environments :)

---

_Comment by @delfick on 2024-07-29 13:47_

I would definitely say that having the ability to constrain what packages are used in an isolated build would be very useful and welcome.

---

_Comment by @henryiii on 2024-07-29 14:11_

It is extremely important to be able to constrain build environments. Whether it's the same variable or not, but it's important to be able to do it! It's used all the time with PIP_CONSTRAINTS.

---

_Comment by @charliermarsh on 2024-07-29 14:24_

Agreed. Somewhat undecided on whether it should be coupled to `--constraints`, since those are typically used to constrain the output resolution, and constraining the build environment is pretty different. Ultimately, though, we'll probably just do that.

---

_Comment by @charliermarsh on 2024-07-29 14:24_

Will track here: https://github.com/astral-sh/uv/issues/5561

---

_Closed by @charliermarsh on 2024-07-29 14:24_

---
