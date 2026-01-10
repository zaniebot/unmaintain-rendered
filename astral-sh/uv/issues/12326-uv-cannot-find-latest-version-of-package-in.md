---
number: 12326
title: uv cannot find latest version of package in private index
type: issue
state: open
author: mbUSC
labels:
  - question
  - external
assignees: []
created_at: 2025-03-20T00:37:56Z
updated_at: 2025-04-07T17:41:44Z
url: https://github.com/astral-sh/uv/issues/12326
synced_at: 2026-01-10T01:25:18Z
---

# uv cannot find latest version of package in private index

---

_Issue opened by @mbUSC on 2025-03-20 00:37_

### Summary

I have a package hosted on a private index. There are multiple versions of the package on the index.
I am able to install the latest published version using `pip install --index-url <index_url> <pkg>==<version>`, but `uv pip install --index-url <index_url> <pkg>==<version>` fails

### Platform

Darwin 24.4.0 arm64

### Version

uv 0.6.8

### Python version

Python 3.13.0

---

_Label `bug` added by @mbUSC on 2025-03-20 00:37_

---

_Comment by @charliermarsh on 2025-03-20 00:38_

Does the index send down caching headers? Have you tried with `--refresh`?

---

_Comment by @mbUSC on 2025-03-21 20:54_

@charliermarsh I tried both with `--refresh` and also ran `uv cache clean` beforehand, but I'm getting the same error :( 

---

_Comment by @zanieb on 2025-03-21 21:02_

Can you share `-vv` logs?

---

_Comment by @mbUSC on 2025-03-22 06:20_

@zanieb Sure

log (redacted):
```
DEBUG uv 0.6.8 (Homebrew 2025-03-18)
DEBUG Searching for default Python interpreter in virtual environments
TRACE Querying interpreter executable at /Users/jd/projects/sample-project/.venv/bin/python3
DEBUG Found `cpython-3.13.0-macos-aarch64-none` at `/Users/jd/projects/sample-project/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.13.0 environment at: .venv
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: my-package==1.3.3
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.13
DEBUG Solving with target Python version: >=3.13
TRACE Assigned packages:
TRACE Chose package for decision: root. remaining choices:
DEBUG Adding direct dependency: my-package>=1.3.3, <1.3.3+
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: my-package. remaining choices:
TRACE Fetching metadata for my-package from https://pypi.internal-index.com/simple/my-package/
TRACE No cache entry exists for /Users/jd/.cache/uv/simple-v15/index/19c87f45b9925eba/my-package.rkyv
DEBUG No cache entry for: https://pypi.internal-index.com/simple/my-package/
TRACE Sending fresh GET request for https://pypi.internal-index.com/simple/my-package/
TRACE Handling request for https://pypi.internal-index.com/simple/my-package/
TRACE Request for https://pypi.internal-index.com/simple/my-package/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.internal-index.com/simple/my-package/
TRACE Attempting unauthenticated request for https://pypi.internal-index.com/simple/my-package/
TRACE Cached request https://pypi.internal-index.com/simple/my-package/ is storable because its response has an 'max-age' cache-control directive
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
TRACE Received package metadata for: my-package
DEBUG Searching for a compatible version of my-package (>=1.3.3, <1.3.3+)
TRACE Selecting candidate for my-package with range >=1.3.3, <1.3.3+ with 57 remote versions
TRACE Selecting candidate for my-package with range >=1.3.3, <1.3.3+ with 57 remote versions
TRACE Exhausted all candidates for package my-package with range >=1.3.3, <1.3.3+ after 0 steps
TRACE Exhausted all candidates for package my-package with range >=1.3.3, <1.3.3+ after 0 steps
DEBUG No compatible version found for: my-package
TRACE Resolver derivation tree before reduction
term root==0a0.dev0
  root==0a0.dev0 depends on my-package==1.3.3
  no versions of my-package==1.3.3
TRACE Resolver derivation tree after reduction
term root==0a0.dev0
  root==0a0.dev0 depends on my-package==1.3.3
  no versions of my-package==1.3.3
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of my-package==1.3.3 and you require my-package==1.3.3, we can conclude that your requirements are unsatisfiable.

      hint: `my-package` was found on https://pypi.internal-index.com/simple/, but not at the requested version (my-package==1.3.3). A compatible version may be
      available on a subsequent index (e.g., https://pypi.org/simple). By default, uv will only consider versions that are published on the first index
      that contains a given package, to avoid dependency confusion attacks. If all indexes are equally trusted, use `--index-strategy unsafe-best-match` to
      consider all versions from all indexes, regardless of the order in which they were defined.
DEBUG Released lock at `/Users/jd/projects/sample-project/.venv/.lock`
```

It looks like the issue is here:
```
WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`
```

But I don't understand why it's failing to parse the version field.
Idk if it matters, but this package that I'm trying to install was packaged using poetry

---

_Comment by @mbUSC on 2025-03-25 14:04_

@zanieb @charliermarsh Is this a bug?
I think it should be able to parse `pyproject.toml` for packages generated by poetry, but I just want to confirm

---

_Comment by @zanieb on 2025-04-01 19:15_

> WARN Skipping file for my-package: Failed to parse `requires-python`: `^3.10`

That's not a valid dependency specifier. See:

- https://packaging.python.org/en/latest/specifications/core-metadata/#requires-python
- https://packaging.python.org/en/latest/specifications/version-specifiers/

You'll need to open an issue with Poetry. You should use `>=3.10,<3.11` instead.

---

_Label `bug` removed by @zanieb on 2025-04-01 19:15_

---

_Label `question` added by @zanieb on 2025-04-01 19:15_

---

_Label `external` added by @zanieb on 2025-04-01 19:15_

---

_Comment by @mbUSC on 2025-04-07 17:41_

@zanieb Got it. 
It would be helpful though to have an actual error message by default (without the need of specifying `-vv`).
Most users (like myself) wouldn't know about `-vv`, so can be very tricky to debug

---
