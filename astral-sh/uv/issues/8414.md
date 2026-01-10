```yaml
number: 8414
title: UV editable install in offline mode not working with Hatchling installed (system-wide)
type: issue
state: closed
author: ion-elgreco
labels:
  - error messages
  - needs-mre
assignees: []
created_at: 2024-10-21T09:47:40Z
updated_at: 2024-10-28T23:01:08Z
url: https://github.com/astral-sh/uv/issues/8414
synced_at: 2026-01-10T04:36:20Z
```

# UV editable install in offline mode not working with Hatchling installed (system-wide)

---

_Issue opened by @ion-elgreco on 2024-10-21 09:47_

I am on the latest UV, trying to install a package as editable:

`uv pip install . --no-deps --offline`

However I get an error message that `hatchling` is not available in the cache. I've `hatchling` installed though in the venv and in the systemwide installation

I guess the question, where is UV looking for hatchling

---

_Comment by @ion-elgreco on 2024-10-21 11:27_

Ok a dependency was missing for hatchling, so that's why it failed. But the error message could have probably shown that better :)

---

_Comment by @zanieb on 2024-10-21 13:51_

I don't think I'll be able to improve this without a minimal reproducible example so I can test improvements.

---

_Label `error messages` added by @zanieb on 2024-10-21 13:51_

---

_Label `needs-mre` added by @charliermarsh on 2024-10-21 14:50_

---

_Comment by @ion-elgreco on 2024-10-21 17:47_

@zanieb this should be it

```
mkdir uvtest
cd uvtest
uv init
uv venv
curl -LO https://files.pythonhosted.org/packages/0c/8b/90e80904fdc24ce33f6fc6f35ebd2232fe731a8528a22008458cf197bc4d/hatchling-1.25.0-py3-none-any.whl
uv pip install hatchling-1.25.0-py3-none-any.whl  --no-deps
uv pip install . --no-deps --offline
```

---

_Comment by @zanieb on 2024-10-21 18:32_

Unfortunately that ran without problems for me.

If I add `UV_CACHE_DIR=/tmp/test` to the last invocation I get the failure

```
Installed 1 package in 2ms
 + hatchling==1.25.0 (from file:///Users/zb/workspace/uv/uvtest/hatchling-1.25.0-py3-none-any.whl)
Resolved 1 package in 0.65ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: uvtest @ file:///Users/zb/workspace/uv/uvtest
  Caused by: Failed to resolve requirements from `setup.py` build
  Caused by: No solution found when resolving: `setuptools>=40.8.0`
  Caused by: Because setuptools was not found in the cache and you require setuptools>=40.8.0, we can conclude that your requirements are unsatisfiable.

hint: Packages were unavailable because the network was disabled. When the network is disabled, registry packages may only be read from the cache.
```

Is this what you encountered?

---

_Comment by @zanieb on 2024-10-21 18:33_

Or was it

```
❯ mkdir uvtest
cd uvtest
uv init --lib
uv venv
curl -LO https://files.pythonhosted.org/packages/0c/8b/90e80904fdc24ce33f6fc6f35ebd2232fe731a8528a22008458cf197bc4d/hatchling-1.25.0-py3-none-any.whl
uv pip install hatchling-1.25.0-py3-none-any.whl  --no-deps
UV_CACHE_DIR=/tmp/cache uv pip install . --no-deps --offline
Initialized project `uvtest`
Using CPython 3.12.6
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 84077  100 84077    0     0  1088k      0 --:--:-- --:--:-- --:--:-- 1094k
Resolved 1 package in 0.96ms
Prepared 1 package in 7ms
Installed 1 package in 2ms
 + hatchling==1.25.0 (from file:///Users/zb/workspace/uv/uvtest/hatchling-1.25.0-py3-none-any.whl)
Resolved 1 package in 0.61ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: uvtest @ file:///Users/zb/workspace/uv/uvtest
  Caused by: Failed to resolve requirements from `build-system.requires`
  Caused by: No solution found when resolving: `hatchling`
  Caused by: Because hatchling was not found in the cache and you require hatchling, we can conclude that your requirements are unsatisfiable.

hint: Packages were unavailable because the network was disabled. When the network is disabled, registry packages may only be read from the cache.
```

---

_Comment by @ion-elgreco on 2024-10-21 18:38_

> Or was it
> 
> ```
> ❯ mkdir uvtest
> cd uvtest
> uv init --lib
> uv venv
> curl -LO https://files.pythonhosted.org/packages/0c/8b/90e80904fdc24ce33f6fc6f35ebd2232fe731a8528a22008458cf197bc4d/hatchling-1.25.0-py3-none-any.whl
> uv pip install hatchling-1.25.0-py3-none-any.whl  --no-deps
> UV_CACHE_DIR=/tmp/cache uv pip install . --no-deps --offline
> Initialized project `uvtest`
> Using CPython 3.12.6
> Creating virtual environment at: .venv
> Activate with: source .venv/bin/activate
>   % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
>                                  Dload  Upload   Total   Spent    Left  Speed
> 100 84077  100 84077    0     0  1088k      0 --:--:-- --:--:-- --:--:-- 1094k
> Resolved 1 package in 0.96ms
> Prepared 1 package in 7ms
> Installed 1 package in 2ms
>  + hatchling==1.25.0 (from file:///Users/zb/workspace/uv/uvtest/hatchling-1.25.0-py3-none-any.whl)
> Resolved 1 package in 0.61ms
> error: Failed to prepare distributions
>   Caused by: Failed to fetch wheel: uvtest @ file:///Users/zb/workspace/uv/uvtest
>   Caused by: Failed to resolve requirements from `build-system.requires`
>   Caused by: No solution found when resolving: `hatchling`
>   Caused by: Because hatchling was not found in the cache and you require hatchling, we can conclude that your requirements are unsatisfiable.
> 
> hint: Packages were unavailable because the network was disabled. When the network is disabled, registry packages may only be read from the cache.
> ```

Yes this is it, so UV didn't highlight that hatchling has an incomplete installation.

Silly me only pulled hatchling without the deps but it confused me because hatchling is installed, only after installing the actual required deps for it it worked

---

_Comment by @zanieb on 2024-10-21 18:46_

I don't think this is a dependency of hatchling missing, I think this is just how the cache works. So my latest invocation is:

```
mkdir uvtest
cd uvtest
uv init --lib
uv venv
curl -LO https://files.pythonhosted.org/packages/0c/8b/90e80904fdc24ce33f6fc6f35ebd2232fe731a8528a22008458cf197bc4d/hatchling-1.25.0-py3-none-any.whl
UV_CACHE_DIR=/tmp/cache uv pip install hatchling-1.25.0-py3-none-any.whl  --no-deps
echo "hatchling==1.25.0" > constraints.txt
UV_CACHE_DIR=/tmp/cache uv pip install . --no-deps --offline --build-constraint constraints.txt -v
```

And we have a cache miss for hatchling in the logs:

```
Initialized project `uvtest`
Using CPython 3.12.6
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 84077  100 84077    0     0  1348k      0 --:--:-- --:--:-- --:--:-- 1368k
Resolved 1 package in 0.53ms
Prepared 1 package in 5ms
Installed 1 package in 2ms
 + hatchling==1.25.0 (from file:///Users/zb/workspace/uv/uvtest/hatchling-1.25.0-py3-none-any.whl)
DEBUG uv 0.4.22 (34be3af84 2024-10-15)
DEBUG Searching for default Python interpreter in system path
DEBUG Found `cpython-3.12.6-macos-aarch64-none` at `/Users/zb/workspace/uv/uvtest/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.6 environment at .venv
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: file:///Users/zb/workspace/uv/uvtest
DEBUG Using request timeout of 30s
DEBUG Found PEP 621 metadata for /Users/zb/workspace/uv/uvtest in `pyproject.toml` (uvtest)
DEBUG Solving with installed Python version: 3.12.6
DEBUG Solving with target Python version: >=3.12.6
DEBUG Adding direct dependency: uvtest*
DEBUG Searching for a compatible version of uvtest @ file:///Users/zb/workspace/uv/uvtest (*)
DEBUG Found static `pyproject.toml` for: uvtest @ file:///Users/zb/workspace/uv/uvtest
DEBUG Project is contained in non-workspace project: `/Users/zb/workspace/uv`
DEBUG No workspace root found, using project root
DEBUG Tried 1 versions: uvtest 1
DEBUG Split specific environment resolution took 0.000s
Resolved 1 package in 0.62ms
DEBUG Identified uncached distribution: uvtest @ file:///Users/zb/workspace/uv/uvtest
DEBUG Unnecessary package: hatchling==1.25.0 (from file:///Users/zb/workspace/uv/uvtest/hatchling-1.25.0-py3-none-any.whl)
DEBUG Acquired lock for `/tmp/cache/sdists-v4/path/f5b78d58f7b0f0fa`
DEBUG Building: uvtest @ file:///Users/zb/workspace/uv/uvtest
DEBUG Project is contained in non-workspace project: `/Users/zb/workspace/uv`
DEBUG No workspace root found, using project root
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.6
DEBUG Solving with target Python version: >=3.12.6
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: hatchling==1.25.0
DEBUG No cache entry for: https://pypi.org/simple/hatchling/
DEBUG Searching for a compatible version of hatchling (==1.25.0)
DEBUG No compatible version found for: hatchling
DEBUG Released lock at `/tmp/cache/sdists-v4/path/f5b78d58f7b0f0fa/.lock`
DEBUG Released lock at `/Users/zb/workspace/uv/uvtest/.venv/.lock`
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: uvtest @ file:///Users/zb/workspace/uv/uvtest
  Caused by: Failed to resolve requirements from `build-system.requires`
  Caused by: No solution found when resolving: `hatchling`
  Caused by: Because hatchling was not found in the cache and you require hatchling==1.25.0, we can conclude that your requirements are unsatisfiable.

hint: Packages were unavailable because the network was disabled. When the network is disabled, registry packages may only be read from the cache.
```

Why this is the case, I'm not quite sure as the cache does have the wheel in it

```
❯ tree /tmp/cache
/tmp/cache
├── CACHEDIR.TAG
├── archive-v0
│   └── 5vaoqNO-RtivDsiYM2uf-
│       ├── hatchling
│       │   ├── __about__.py
│       │   ├── __init__.py
│       │   ├── __main__.py
│       │   ├── bridge
│       │   │   ├── __init__.py
│       │   │   └── app.py
│       │   ├── build.py
│       │   ├── builders
│       │   │   ├── __init__.py
│       │   │   ├── app.py
│       │   │   ├── binary.py
│       │   │   ├── config.py
│       │   │   ├── constants.py
│       │   │   ├── custom.py
│       │   │   ├── hooks
│       │   │   │   ├── __init__.py
│       │   │   │   ├── custom.py
│       │   │   │   ├── plugin
│       │   │   │   │   ├── __init__.py
│       │   │   │   │   ├── hooks.py
│       │   │   │   │   └── interface.py
│       │   │   │   └── version.py
│       │   │   ├── macos.py
│       │   │   ├── plugin
│       │   │   │   ├── __init__.py
│       │   │   │   ├── hooks.py
│       │   │   │   └── interface.py
│       │   │   ├── sdist.py
│       │   │   ├── utils.py
│       │   │   └── wheel.py
│       │   ├── cli
│       │   │   ├── __init__.py
│       │   │   ├── build
│       │   │   │   └── __init__.py
│       │   │   ├── dep
│       │   │   │   └── __init__.py
│       │   │   ├── metadata
│       │   │   │   └── __init__.py
│       │   │   └── version
│       │   │       └── __init__.py
│       │   ├── dep
│       │   │   ├── __init__.py
│       │   │   └── core.py
│       │   ├── licenses
│       │   │   ├── __init__.py
│       │   │   ├── parse.py
│       │   │   └── supported.py
│       │   ├── metadata
│       │   │   ├── __init__.py
│       │   │   ├── core.py
│       │   │   ├── custom.py
│       │   │   ├── plugin
│       │   │   │   ├── __init__.py
│       │   │   │   ├── hooks.py
│       │   │   │   └── interface.py
│       │   │   ├── spec.py
│       │   │   └── utils.py
│       │   ├── ouroboros.py
│       │   ├── plugin
│       │   │   ├── __init__.py
│       │   │   ├── exceptions.py
│       │   │   ├── manager.py
│       │   │   ├── specs.py
│       │   │   └── utils.py
│       │   ├── py.typed
│       │   ├── utils
│       │   │   ├── __init__.py
│       │   │   ├── constants.py
│       │   │   ├── context.py
│       │   │   └── fs.py
│       │   └── version
│       │       ├── __init__.py
│       │       ├── core.py
│       │       ├── scheme
│       │       │   ├── __init__.py
│       │       │   ├── plugin
│       │       │   │   ├── __init__.py
│       │       │   │   ├── hooks.py
│       │       │   │   └── interface.py
│       │       │   └── standard.py
│       │       └── source
│       │           ├── __init__.py
│       │           ├── code.py
│       │           ├── env.py
│       │           ├── plugin
│       │           │   ├── __init__.py
│       │           │   ├── hooks.py
│       │           │   └── interface.py
│       │           └── regex.py
│       └── hatchling-1.25.0.dist-info
│           ├── METADATA
│           ├── RECORD
│           ├── WHEEL
│           ├── entry_points.txt
│           └── licenses
│               └── LICENSE.txt
├── builds-v0
├── interpreter-v2
│   └── e5392d251bd8ff84.msgpack
├── sdists-v4
│   └── path
│       └── f5b78d58f7b0f0fa
│           ├── revision.rev
│           └── uQnvsuNfXwujpeG_rnTra
└── wheels-v2
    └── url
        └── 644d060f6d931302
            └── hatchling
                ├── hatchling-1.25.0-py3-none-any -> /tmp/cache/archive-v0/5vaoqNO-RtivDsiYM2uf-
                └── hatchling-1.25.0-py3-none-any.rev
```

Perhaps because it was installed from a local file instead of the registry? I presume @charliermarsh would know.

---

_Comment by @ion-elgreco on 2024-10-21 18:51_

@zanieb important to note the install happens in a system with no network access.

So I had prefecthed the wheel beforehand

---

_Comment by @charliermarsh on 2024-10-28 21:15_

I wrote up an explanation of the general problem here: https://github.com/astral-sh/uv/issues/8647#issuecomment-2442634432. I think we should consider closing both issues in favor of https://github.com/astral-sh/uv/issues/7052.

---

_Comment by @zanieb on 2024-10-28 21:17_

Hm but that won't solve the `pip install` workflow here, right?

---

_Comment by @charliermarsh on 2024-10-28 21:21_

No, but this can be solved separately by writing a `constraints.txt` like:

```
hatchling @ ./hatchling-1.25.0-py3-none-any.whl
```

And then: `UV_CACHE_DIR=/tmp/cache uv pip install . --no-deps --offline --build-constraint constraints.txt`.

---

_Comment by @charliermarsh on 2024-10-28 21:22_

(Though `hatchling-1.25.0-py3-none-any.whl` shouldn't be installed with `--no-deps` -- it has a bunch of deps that need to be cached.)

---

_Comment by @charliermarsh on 2024-10-28 21:24_

Using the `--build-constraint` tells uv that you want to use this local `hatchling` rather than finding it from PyPI.

---

_Comment by @charliermarsh on 2024-10-28 23:01_

I think `--build-constraint` is a good solution here.

---

_Closed by @charliermarsh on 2024-10-28 23:01_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-28 23:01_

---
