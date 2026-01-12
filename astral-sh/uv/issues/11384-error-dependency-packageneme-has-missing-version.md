```yaml
number: 11384
title: "Error `Dependency `packageneme` has missing `version` field but has more than one matching package` when using conflicting optional dependencies."
type: issue
state: closed
author: jenshnielsen
labels:
  - bug
assignees: []
created_at: 2025-02-10T13:57:17Z
updated_at: 2025-02-10T20:21:03Z
url: https://github.com/astral-sh/uv/issues/11384
synced_at: 2026-01-12T16:00:35Z
```

# Error `Dependency `packageneme` has missing `version` field but has more than one matching package` when using conflicting optional dependencies.

---

_@jenshnielsen_

### Summary

I have been trying to configure uv to allow me to have two sets of conflicting optional dependencies to allow me to easily switch between 
having specific dependencies installed from pypi or editable from a local folder. However, when I try to do this, I end up with an error of the form 

```
error: Failed to parse `uv.lock`
  Caused by: Dependency `qcodes` has missing `version` field but has more than one matching package
```
Which I can only work around by deleting the uv.lock file.


To reproduce: Have a project with a dynamic version field in pyproject.toml checked out in a subfolder of the current folder.
In this case I use [QCoDeS](https://github.com/microsoft/Qcodes.git) which I maintain but I can also reproduce the issue with other projects.

```
uv init demo
```

Add 
```
[project.optional-dependencies]
editable = [
    "qcodes>=0.50.1"
    ]
pypi = [
    "qcodes>=0.50.1"
    ]

[tool.uv]

conflicts = [[
        { extra = "editable" },
        { extra = "pypi" },
         ]]

[tool.uv.sources]


qcodes = [
    { path = "../qcodes", editable = true , extra = "editable"},
    { index="pypi" , extra = "pypi"}
    ]


[[tool.uv.index]]
name = "pypi"
url = "https://pypi.org/simple/"
explicit = true

```
To the demo projects pyproject.toml file

Run 

```
uv sync --extra pypi
```
Correctly installs all dependencies as expected from pypi.

Howeverm when then running.

```
uv sync --extra editable  --upgrade
```

I get 

```
error: Failed to parse `uv.lock`
  Caused by: Dependency `qcodes` has missing `version` field but has more than one matching package
```

Removing the uv.lock file and I can correctly install the editable version.

Now running 

```
uv sync --extra pypi  --upgrade
```
results in the same error which can then be worked around by removing the uv.lock file













### Platform

Window 11 24H2

### Version

uv 0.5.29 (ca73c4754 2025-02-05)

### Python version

Python 3.13 but also reproduced using 3.11

---

_Label `bug` added by @jenshnielsen on 2025-02-10 13:57_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-10 19:22_

---

_Comment by @charliermarsh on 2025-02-10 20:16_

I think this is just an oversight. Fixed in https://github.com/astral-sh/uv/pull/11400.

---

_Closed by @charliermarsh on 2025-02-10 20:21_

---
