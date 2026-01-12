```yaml
number: 14591
title: "sync: `--find-links` ignores 'linux'-tagged wheel"
type: issue
state: closed
author: craigds
labels:
  - question
assignees: []
created_at: 2025-07-14T00:04:22Z
updated_at: 2025-07-14T01:58:31Z
url: https://github.com/astral-sh/uv/issues/14591
synced_at: 2026-01-12T16:01:52Z
```

# sync: `--find-links` ignores 'linux'-tagged wheel

---

_@craigds_

### Summary

sync seems unable to find our specially-built `lxml` wheel, and is instead attempting to build lxml from an sdist (we ignore manylinux wheels from pypi, via the `no-manylinux` package):

```
root@72f872e12dda:/app# uv sync -vvv --find-links /build-cache/python/wheelhouse/ --package lxml --no-index --frozen --inexact --refresh
DEBUG uv 0.7.20
DEBUG Found project root: `/app`
DEBUG No workspace root found, using project root
TRACE Checking lock for `/app` at `/tmp/uv-1c83b73deef05048.lock`
DEBUG Acquired lock for `/app`
DEBUG No Python version file found in workspace: /app
DEBUG Using Python request `>=3.10` from `requires-python` metadata
DEBUG Checking for Python environment at `.venv`
TRACE Querying interpreter executable at /app/.venv/bin/python3
DEBUG The project environment's Python version satisfies the request: `Python >=3.10`
TRACE The project environment's Python version meets the Python requirement: `>=3.10`
DEBUG Released lock at `/tmp/uv-1c83b73deef05048.lock`
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found 64 packages in `--find-links` entry: file:///build-cache/python/wheelhouse
DEBUG Must revalidate requirement: lxml
DEBUG Unnecessary package: no-manylinux==3.0.0
TRACE Checking lock for `/root/.cache/uv/sdists-v9/index/a661ae14c791e62e/lxml/5.4.0` at `/root/.cache/uv/sdists-v9/index/a661ae14c791e62e/lxml/5.4.0/.lock`
DEBUG Acquired lock for `/root/.cache/uv/sdists-v9/index/a661ae14c791e62e/lxml/5.4.0`
TRACE Cached request https://devpi.kx.gd/root/pypi/+f/d12/832e1dbea4be2/lxml-5.4.0.tar.gz is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://devpi.kx.gd/root/pypi/+f/d12/832e1dbea4be2/lxml-5.4.0.tar.gz
   Building lxml==5.4.0
DEBUG Building: lxml==5.4.0
DEBUG Not using uv build backend direct build of lxml==5.4.0, no pyproject.toml: TOML parse error at line 1, column 1
  |
1 | [build-system]
  | ^
missing field `project`

TRACE Requirements for direct build not matched: lxml==5.4.0
DEBUG Assessing Python executable as base candidate: /app/.venv/bin/python3
DEBUG Assessing Python executable as base candidate: /app/.venv/bin/python
DEBUG Assessing Python executable as base candidate: /usr/bin/python3
DEBUG Reusing existing build environment for: lxml==5.4.0
DEBUG Using base executable for virtual environment: /app/.venv/bin/python3
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.10.12
DEBUG Solving with target Python version: >=3.10.12
TRACE Assigned packages:
TRACE Chose package for decision: root. remaining choices:
DEBUG Adding direct dependency: cython>=3.0.11, <3.1.0
DEBUG Adding direct dependency: setuptools*
DEBUG Adding direct dependency: wheel*
INFO add_decision: Id::<PubGrubPackage>(0) @ 0a0.dev0 without checking dependencies
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: cython. remaining choices: wheel, setuptools
TRACE Received package metadata for: cython
DEBUG Searching for a compatible version of cython (>=3.0.11, <3.1.0)
TRACE Selecting candidate for cython with range >=3.0.11, <3.1.0 with 0 remote versions
DEBUG No compatible version found for: cython
INFO Start conflict resolution because incompat satisfied:
   cython >=3.0.11, <3.1.0 is forbidden
TRACE Received package metadata for: setuptools
TRACE Received package metadata for: wheel
TRACE Selecting candidate for setuptools with range * with 1 remote versions
TRACE Found candidate for package setuptools with range * after 1 steps: 80.9.0 version
TRACE Returning candidate for package setuptools with range * after 1 steps
INFO prior cause: root ==0a0.dev0 is forbidden
TRACE Received built distribution metadata for: setuptools==80.9.0
DEBUG Released lock at `/root/.cache/uv/sdists-v9/index/a661ae14c791e62e/lxml/5.4.0/.lock`
  × Failed to download and build `lxml==5.4.0`
  ├─▶ Failed to resolve requirements from `build-system.requires`
  ├─▶ No solution found when resolving: `cython>=3.0.11, <3.1.0`, `setuptools`, `wheel`
TRACE Resolver derivation tree before reduction
term root==0a0.dev0
  root==0a0.dev0 depends on cython>=3.0.11, <3.1.0
  cython not found in the provided package locations
TRACE Resolver derivation tree after reduction
term root==0a0.dev0
  root==0a0.dev0 depends on cython>=3.0.11, <3.1.0
  cython not found in the provided package locations
  ╰─▶ Because cython was not found in the provided package locations and you require cython>=3.0.11,<3.1.0, we can conclude that your requirements are
      unsatisfiable.
DEBUG Released lock at `/app/.venv/.lock`
```




The wheel is present in that directory. uv is able to find pure-python wheels in that directory just fine, but seems to trip on wheels with the `linux` tag.


### Steps to reproduce

I have pushed a self-contained dockerfile repo here, including the wheels (both amd64 and arm64 variants) involved: https://github.com/craigds/uv-sync-bug

Just build the repo and it demonstrates the issue; uv fails to find those wheels and falls back to building from an sdist (which fails due to missing build deps, but that's not the issue)

### Platform

ubuntu or debian, tried amd64 and aarch64

### Version

0.7.20

### Python version

3.10.12

---

_Label `bug` added by @craigds on 2025-07-14 00:04_

---

_Renamed from "sync: `--find-links` ignores 'linux' wheel" to "sync: `--find-links` ignores 'linux'-tagged wheel" by @craigds on 2025-07-14 00:05_

---

_Comment by @craigds on 2025-07-14 00:07_

oh, I should note: uv _does_ install this wheel just fine if I pass the filename to `uv pip install`:

```
root@72f872e12dda:/app# uv pip install /build-cache/python/wheelhouse/lxml-5.4.0-cp310-cp310-linux_aarch64.whl
Resolved 1 package in 5ms
Prepared 1 package in 59ms
Installed 1 package in 4ms
 + lxml==5.4.0 (from file:///build-cache/python/wheelhouse/lxml-5.4.0-cp310-cp310-linux_aarch64.whl)
```

So, I don't think there's anything wrong with the wheel, just that sync is ignoring it in favour of the sdist for some reason.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-14 00:57_

---

_Comment by @charliermarsh on 2025-07-14 01:04_

Using `--frozen` means that uv will use the sources recorded in your lockfile as they exist at the start of the command, verbatim. So I don't think the `--find-links` entry is being consulted at all. You should incorporate the `--find-links` when you lock, so that it's encoded in the lockfile.

---

_Label `bug` removed by @charliermarsh on 2025-07-14 01:05_

---

_Label `question` added by @charliermarsh on 2025-07-14 01:05_

---

_Comment by @craigds on 2025-07-14 01:32_

interesting, thanks for the response. This seems to be a problem even without `--frozen` though (interestingly, it's not trying to build the wheel now though):

```
 > [7/7] RUN uv sync -vvv --no-index --find-links=/wheelhouse/ --inexact --package lxml --all-groups --refresh:
0.115 DEBUG uv 0.7.20
0.120 DEBUG Found workspace root: `/app`
0.120 TRACE Discovering workspace members for: `/app`
0.120 DEBUG Adding root workspace member: `/app`
0.120 error: Package `lxml` not found in workspace
```

(I've updated the linked repro dockerfile to use the above)

I can't immediately think of a way to use uv with our workflow, because I don't think we can add the wheelhouse source to our lockfile:

* the lockfile is generated by a developer before committing
* wheels for all dependencies are built later by CI using `pip wheel`

since the wheel doesnt' exist when the lockfile is generated, we can't insert it in there.

---

_Comment by @charliermarsh on 2025-07-14 01:36_

`--package lxml` means "the package in the workspace named `lxml`".  You can't put arbitrary dependencies there -- it has to be a workspace member. You should omit that argument.

---

_Comment by @craigds on 2025-07-14 01:42_

aha, so `--package` is referring to a subpackage of a monorepo or something like that; not "only try to install/update a specific dependency from my big list of dependencies" 👍 

yes it's working without that option and installing the wheel regardless of the lockfile 🎉 

thanks loads for your help, very much appreciated 💯 

---

_Closed by @craigds on 2025-07-14 01:42_

---

_Comment by @charliermarsh on 2025-07-14 01:58_

No problem, happy to help!

---
