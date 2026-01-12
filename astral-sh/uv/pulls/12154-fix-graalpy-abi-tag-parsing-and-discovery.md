```yaml
number: 12154
title: Fix GraalPy abi tag parsing and discovery
type: pull_request
state: merged
author: timfel
labels:
  - bug
assignees: []
merged: true
base: main
head: tim/fix-graalpy-tag-parsing-and-discovery
created_at: 2025-03-13T19:37:13Z
updated_at: 2025-03-14T07:33:36Z
url: https://github.com/astral-sh/uv/pull/12154
synced_at: 2026-01-12T16:10:08Z
```

# Fix GraalPy abi tag parsing and discovery

---

_@timfel_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

There were no GraalPy binary wheels were available when uv support was added, and thus the abi tag was never tested against actual packages. Now that GraalPy publishes binary wheels to https://www.graalvm.org/python/wheels/ we noticed the abi tag was incorrect and the version info incorrectly determined.

## Test Plan

I tested manually:

```
> target/debug/uv venv --python graalpy testvenv
Using GraalPy 3.11.7 interpreter at: /home/tim/.pyenv/versions/graalpy-24.1.1/bin/graalpy
Creating virtual environment at: testvenv
Activate with: source testvenv/bin/activate
> cat <<EOF> uv.toml
> [[index]]
> url = "https://www.graalvm.org/python/wheels/"
> EOF
> target/debug/uv -v pip install psutil
warning: Found both a `uv.toml` file and a `[tool.uv]` section in an adjacent `pyproject.toml`. The `[tool.uv]` section will be ignored in favor of the `uv.toml` file.
DEBUG uv 0.6.6+3 (be8725553 2025-03-13)
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `graalpy-3.11.7-linux-x86_64-gnu` at `/home/tim/dev/uv/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.11.7 environment at: .venv
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: psutil
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.11.7
DEBUG Solving with target Python version: >=3.11.7
DEBUG Adding direct dependency: psutil*
DEBUG Found fresh response for: https://www.graalvm.org/python/wheels/psutil/
DEBUG Searching for a compatible version of psutil (*)
DEBUG Selecting: psutil==5.9.8 [compatible] (psutil-5.9.8-graalpy311-graalpy241_311_native-manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_28_x86_64.whl)
DEBUG No cache entry for: https://gds.oracle.com/download/graalpy-wheels/psutil-5.9.8-graalpy311-graalpy241_311_native-manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_28_x86_64.whl
DEBUG Tried 1 versions: psutil 1
DEBUG marker environment resolution took 0.968s
Resolved 1 package in 971ms
DEBUG Identified uncached distribution: psutil==5.9.8
DEBUG No cache entry for: https://gds.oracle.com/download/graalpy-wheels/psutil-5.9.8-graalpy311-graalpy241_311_native-manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_28_x86_64.whl
Prepared 1 package in 268ms
Installed 1 package in 28ms
 + psutil==5.9.8
DEBUG Released lock at `/home/tim/dev/uv/.venv/.lock`
```

---

_@charliermarsh approved on 2025-03-13 19:39_

Thank you, this is very welcome.

---

_Label `bug` added by @charliermarsh on 2025-03-13 23:48_

---

_Merged by @charliermarsh on 2025-03-13 23:55_

---

_Closed by @charliermarsh on 2025-03-13 23:55_

---

_Branch deleted on 2025-03-14 07:33_

---

_Comment by @timfel on 2025-03-14 07:33_

Thanks @charliermarsh for taking care of packse and merging 🙇‍♂️ 

---
