---
number: 14461
title: build-constraint-dependencies specific to an individual dependency
type: issue
state: open
author: callegar
labels:
  - enhancement
assignees: []
created_at: 2025-07-04T15:33:56Z
updated_at: 2025-07-07T17:01:06Z
url: https://github.com/astral-sh/uv/issues/14461
synced_at: 2026-01-07T13:12:18-06:00
---

# build-constraint-dependencies specific to an individual dependency

---

_Issue opened by @callegar on 2025-07-04 15:33_

### Summary

I have a project that requires `scikits-odes` and that must be deployable on debian.

The `scikits-odes` package is tightly coupled to the sundials library, so you must pick an older version of `scikits-odes` to be able to build it on debian and specifically version 2.7.0.

This versions builds without errors only with cython < 3 and setuptools < 60.  Hence in order to let my project correctly build its virtual environment, I need to specify:
```
[tool.uv]
 build-constraint-dependencies = [
  "setuptools<60",
  "cython<3",
]
```
in my `pyproject.toml`. However, this adds the build constraint to *everything* and not just to `scikits-odes`. Is there a provision to be able to specify build constraints tying them to specific required packages?

### Example

_No response_

---

_Label `enhancement` added by @callegar on 2025-07-04 15:33_

---

_Comment by @callegar on 2025-07-05 06:44_

Specifically: could the option be given to make `build-constraint-dependencies` a table, where the constraints are expressed per package? E.g.:
```
[toml.uv."build-constraint-dependencies"]
"scikits-odes" = [
  "setuptools<60",
  "cython<3",
]

---

_Comment by @konstin on 2025-07-07 09:37_

> in my `pyproject.toml`. However, this adds the build constraint to _everything_ and not just to `scikits-odes`. Is there a provision to be able to specify build constraints tying them to specific required packages?

Currently, we don't have a way to specify that yet.

---

_Comment by @zanieb on 2025-07-07 17:01_

I think I'd be supportive of a `build-constraint-dependencies-package = {package = [values]}`

---
