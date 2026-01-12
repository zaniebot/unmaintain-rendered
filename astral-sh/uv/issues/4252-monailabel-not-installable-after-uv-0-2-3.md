```yaml
number: 4252
title: monailabel not installable after uv 0.2.3
type: issue
state: closed
author: StellarStorm
labels:
  - bug
assignees: []
created_at: 2024-06-11T20:37:03Z
updated_at: 2024-06-11T21:40:10Z
url: https://github.com/astral-sh/uv/issues/4252
synced_at: 2026-01-12T15:58:49Z
```

# monailabel not installable after uv 0.2.3

---

_@StellarStorm_

uv 0.2.3 and later cannot install [monailabel](https://pypi.org/project/monailabel/), but 0.2.2 and earlier can. It is still installable with `pip`.

* Minimum reproducible example:
1. Create new environment
2. Try to install monailabel

```bash
$ conda create --name demo python=3.11

$ conda activate demo

$ uv pip install --dry-run --verbose monailabel
DEBUG Searching for Python interpreter in virtual environments
DEBUG Found CPython 3.11.9 at `/opt/homebrew/Caskroom/mambaforge/base/envs/demo/bin/python3` (conda prefix)
DEBUG Using Python 3.11.9 environment at /opt/homebrew/Caskroom/mambaforge/base/envs/demo/bin/python3
DEBUG Acquired lock for `/opt/homebrew/Caskroom/mambaforge/base/envs/demo`
DEBUG At least one requirement is not satisfied: monailabel
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.11.9
DEBUG Adding direct dependency: monailabel*
DEBUG Found fresh response for: https://pypi.org/simple/monailabel/
DEBUG Searching for a compatible version of monailabel (*)
DEBUG No compatible version found for: monailabel
  × No solution found when resolving dependencies:
  ╰─▶ Because there are no versions of monailabel and you require monailabel, we can conclude that the requirements are unsatisfiable.

$ pip install --dry-run monailabel  # successfully installs
```

Attempting to specify the version to install doesn't work either
```bash
$ uv pip install --dry-run monailabel==0.8.3
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of monailabel==0.8.3 and you require monailabel==0.8.3, we can conclude that the requirements are unsatisfiable.

$ uv pip install --dry-run monailabel==0.8.2
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of monailabel==0.8.2 and you require monailabel==0.8.2, we can conclude that the requirements are unsatisfiable.

$ uv pip install --dry-run monailabel==0.8.0
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of monailabel==0.8.0 and you require monailabel==0.8.0, we can conclude that the requirements are unsatisfiable.
```


* Current uv platform: macOS installed with `brew`. I've also observed this on Linux-based Docker images where uv is installed with `pip`.
* Current uv version: **0.2.10**.



---

_Comment by @zanieb on 2024-06-11 20:40_

Huh thanks for the report that's very weird.

```


❯ RUST_LOG=uv=trace uv pip install --dry-run --verbose monailabel
DEBUG Searching for Python interpreter in virtual environments
TRACE Cached interpreter info for Python 3.12.3, skipping probing: .venv/bin/python3
DEBUG Found CPython 3.12.3 at `/Users/zb/workspace/uv/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.3 environment at .venv/bin/python3
TRACE Checking lock for `.venv`
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: monailabel
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.3
DEBUG Adding direct dependency: monailabel*
TRACE Fetching metadata for monailabel from https://pypi.org/simple/monailabel/
TRACE No cache entry exists for /Users/zb/Library/Caches/uv/simple-v8/pypi/monailabel.rkyv
DEBUG No cache entry for: https://pypi.org/simple/monailabel/
TRACE Sending fresh GET request for https://pypi.org/simple/monailabel/
TRACE Handling request for https://pypi.org/simple/monailabel/
TRACE Request for https://pypi.org/simple/monailabel/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/monailabel/
TRACE Attempting unauthenticated request for https://pypi.org/simple/monailabel/
TRACE cached request https://pypi.org/simple/monailabel/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: monailabel
TRACE selecting candidate for package monailabel with range Range { segments: [(Unbounded, Unbounded)] } with 0 remote versions
TRACE exhausted all candidates for package PackageName("monailabel") with range Range { segments: [(Unbounded, Unbounded)] } after 0 steps
DEBUG Searching for a compatible version of monailabel (*)
TRACE selecting candidate for package monailabel with range Range { segments: [(Unbounded, Unbounded)] } with 0 remote versions
TRACE exhausted all candidates for package PackageName("monailabel") with range Range { segments: [(Unbounded, Unbounded)] } after 0 steps
DEBUG No compatible version found for: monailabel
  × No solution found when resolving dependencies:
  ╰─▶ Because there are no versions of monailabel and you require monailabel, we can conclude that the requirements are unsatisfiable.
```

https://pypi.org/simple/monailabel/ shows packages...

---

_Comment by @charliermarsh on 2024-06-11 21:13_

I can take a look.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-11 21:22_

---

_Comment by @charliermarsh on 2024-06-11 21:23_

Probably something to do with all of the versions having a build tag.

---

_Label `bug` added by @charliermarsh on 2024-06-11 21:30_

---

_Closed by @charliermarsh on 2024-06-11 21:40_

---
