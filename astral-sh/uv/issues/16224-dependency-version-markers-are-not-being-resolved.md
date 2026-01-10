---
number: 16224
title: Dependency version markers are not being resolved as expected
type: issue
state: closed
author: skraeven
labels:
  - question
assignees: []
created_at: 2025-10-10T08:50:01Z
updated_at: 2025-10-28T23:56:54Z
url: https://github.com/astral-sh/uv/issues/16224
synced_at: 2026-01-10T01:26:04Z
---

# Dependency version markers are not being resolved as expected

---

_Issue opened by @skraeven on 2025-10-10 08:50_

### Summary

Given this pyproject.toml:
```
[project]
name = "foo"
version = "0.1.0"
description = "bar"
requires-python = ">=3.12"
dependencies = [
    "htmlmin;python_version<'3.13'",
]
```
running `uv sync` will fail, due to "cgi" which was removed in python 3.13:
```
      ModuleNotFoundError: No module named 'cgi'

      hint: This error likely indicates that `htmlmin@0.1.12` depends on `cgi`, but doesn't declare it as a build dependency. If `htmlmin` is a
      first-party package, consider adding `cgi` to its `build-system.requires`. Otherwise, either add it to your `pyproject.toml` under:

      [tool.uv.extra-build-dependencies]
      htmlmin = ["cgi"]

      or `uv pip install cgi` into the environment and re-run with `--no-build-isolation`.
  help: `htmlmin` (v0.1.12) was included because `foo` (v0.1.0) depends on `htmlmin`
```
One workaround that I found:
```
uv python pin 3.12
uv sync
uv python pin 3.13
uv sync
```
Then sync will complete for 3.13 without issues.

### Platform

Ubuntu 24.04

### Version

0.9.1

### Python version

_No response_

---

_Label `bug` added by @skraeven on 2025-10-10 08:50_

---

_Label `bug` removed by @konstin on 2025-10-21 13:20_

---

_Label `question` added by @konstin on 2025-10-21 13:20_

---

_Comment by @konstin on 2025-10-21 13:21_

To build a lockfile that works for both 3.12 and 3.13, uv needs to know the dependencies of htmlmin. The package doesn't five us this information statically: There's no wheel, no `pyproject.toml` and no modern reliable metadata in the source distribution. For those cases, uv needs to build the package to extract the metadata. This build fails with the current default Python version (3.13), and only succeed when using 3.12 as default Python instead. The best solution to this is htmlmin providing static metadata which skips the build altogether.

A smaller workaround:

```
uv lock -p 3.12
uv sync -p 3.13
```

---

_Closed by @charliermarsh on 2025-10-28 23:56_

---
