---
number: 12549
title: "Can't pin existing version of numpy"
type: issue
state: closed
author: RyanBlace
labels:
  - question
assignees: []
created_at: 2025-03-30T03:19:18Z
updated_at: 2025-03-30T15:31:59Z
url: https://github.com/astral-sh/uv/issues/12549
synced_at: 2026-01-10T01:25:21Z
---

# Can't pin existing version of numpy

---

_Issue opened by @RyanBlace on 2025-03-30 03:19_

### Summary

I can't figure out a shorter way of making this happen, so here it goes:


❯ uv init -p 3.8.20
Initialized project `testuv`


❯ uv add scipy==1.7.3
Using CPython 3.8.20
Creating virtual environment at: .venv
Resolved 3 packages in 308ms
Prepared 2 packages in 9.12s
Installed 2 packages in 248ms
 + numpy==1.22.4
 + scipy==1.7.3


❯ uv add numpy==1.20.0
Resolved 3 packages in 14ms
Uninstalled 1 package in 105ms
Installed 1 package in 124ms
 - numpy==1.22.4
 + numpy==1.20.0


❯ uv add xgboost==2.0.0
Resolved 4 packages in 15ms
Installed 1 package in 12ms
 + xgboost==2.0.0

 
❯ uv tree
Resolved 4 packages in 0.95ms
testuv v0.1.0
├── numpy v1.20.0
├── scipy v1.7.3
│   └── numpy v1.20.0
└── xgboost v2.0.0
    ├── numpy v1.20.0
    └── scipy v1.7.3 (*)
(*) Package tree already displayed


❯ uv remove numpy
Resolved 4 packages in 14ms
Audited 3 packages in 0.01ms

 
❯ uv tree
Resolved 4 packages in 1ms
testuv v0.1.0
├── scipy v1.7.3
│   └── numpy v1.20.0
└── xgboost v2.0.0
    ├── numpy v1.20.0
    └── scipy v1.7.3 (*)
(*) Package tree already displayed


❯ uv add pandas==1.3.5
Resolved 9 packages in 19ms
Installed 4 packages in 298ms
 + pandas==1.3.5
 + python-dateutil==2.9.0.post0
 + pytz==2025.2
 + six==1.17.0


❯ uv tree
Resolved 9 packages in 1ms
testuv v0.1.0
├── pandas v1.3.5
│   ├── numpy v1.20.0
│   ├── python-dateutil v2.9.0.post0
│   │   └── six v1.17.0
│   └── pytz v2025.2
├── scipy v1.7.3
│   └── numpy v1.20.0
└── xgboost v2.0.0
    ├── numpy v1.20.0
    └── scipy v1.7.3 (*)
(*) Package tree already displayed


❯ uv add numpy==1.20.0
  × No solution found when resolving dependencies for split (python_full_version >= '3.10'):
  ╰─▶ Because only the following versions of numpy{python_full_version >= '3.10'} are available:
          numpy{python_full_version >= '3.10'}<=1.21.0
          numpy{python_full_version >= '3.10'}==1.21.1
          numpy{python_full_version >= '3.10'}==1.21.2
          numpy{python_full_version >= '3.10'}==1.21.3
          numpy{python_full_version >= '3.10'}==1.21.4
          numpy{python_full_version >= '3.10'}==1.21.5
          numpy{python_full_version >= '3.10'}==1.21.6
          numpy{python_full_version >= '3.10'}==1.22.0
          numpy{python_full_version >= '3.10'}==1.22.1
          numpy{python_full_version >= '3.10'}==1.22.2
          numpy{python_full_version >= '3.10'}==1.22.3
          numpy{python_full_version >= '3.10'}==1.22.4
          numpy{python_full_version >= '3.10'}==1.23.0
          numpy{python_full_version >= '3.10'}==1.23.1
          numpy{python_full_version >= '3.10'}==1.23.2
          numpy{python_full_version >= '3.10'}==1.23.3
          numpy{python_full_version >= '3.10'}==1.23.4
          numpy{python_full_version >= '3.10'}==1.23.5
          numpy{python_full_version >= '3.10'}==1.24.0
          numpy{python_full_version >= '3.10'}==1.24.1
          numpy{python_full_version >= '3.10'}==1.24.2
          numpy{python_full_version >= '3.10'}==1.24.3
          numpy{python_full_version >= '3.10'}==1.24.4
          numpy{python_full_version >= '3.10'}==1.25.0
          numpy{python_full_version >= '3.10'}==1.25.1
          numpy{python_full_version >= '3.10'}==1.25.2
          numpy{python_full_version >= '3.10'}==1.26.0
          numpy{python_full_version >= '3.10'}==1.26.1
          numpy{python_full_version >= '3.10'}==1.26.2
          numpy{python_full_version >= '3.10'}==1.26.3
          numpy{python_full_version >= '3.10'}==1.26.4
          numpy{python_full_version >= '3.10'}==2.0.0
          numpy{python_full_version >= '3.10'}==2.0.1
          numpy{python_full_version >= '3.10'}==2.0.2
          numpy{python_full_version >= '3.10'}==2.1.0
          numpy{python_full_version >= '3.10'}==2.1.1
          numpy{python_full_version >= '3.10'}==2.1.2
          numpy{python_full_version >= '3.10'}==2.1.3
          numpy{python_full_version >= '3.10'}==2.2.0
          numpy{python_full_version >= '3.10'}==2.2.1
          numpy{python_full_version >= '3.10'}==2.2.2
          numpy{python_full_version >= '3.10'}==2.2.3
          numpy{python_full_version >= '3.10'}==2.2.4
      and pandas==1.3.5 depends on numpy{python_full_version >= '3.10'}>=1.21.0, we can conclude that pandas==1.3.5 depends on numpy>=1.21.0.
      And because your project depends on numpy==1.20.0 and pandas==1.3.5, we can conclude that your project's requirements are unsatisfiable.

      hint: Pre-releases are available for `numpy` in the requested range (e.g., 2.2.0rc1), but pre-releases weren't enabled (try: `--prerelease=allow`)
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.

─────────


As you can see, numpy is currently 1.20.0, but I can't add that to the pyproject.toml.

### Platform

Microsoft Windows [Version 10.0.19045.5608]

### Version

uv 0.6.10 (f2a2d982b 2025-03-25)

### Python version

uv init -p 3.8.20

---

_Label `bug` added by @RyanBlace on 2025-03-30 03:19_

---

_Comment by @charliermarsh on 2025-03-30 15:27_

I think this is right. You've pinned `pandas==1.3.5`, and now you're trying to pin `numpy==1.20.0` on _all_ platforms. But Pandas has `Requires-Dist: numpy (>=1.21.0) ; python_version >= "3.10"`. So now you've asked for an invalid set of requirements for Python 3.10 and later.

`uv tree` shows 1.20.0 because it shows the resolved dependencies for _your_ machine by default. (You can run `uv tree --universal` to view all dependencies for all platforms.) 

---

_Label `bug` removed by @charliermarsh on 2025-03-30 15:27_

---

_Label `question` added by @charliermarsh on 2025-03-30 15:27_

---

_Comment by @charliermarsh on 2025-03-30 15:31_

If you don't care about Python 3.10 or later, you could do something like:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.8"
dependencies = [
    "numpy==1.20.0",
    "pandas==1.3.5",
]

[tool.uv]
environments = ["python_full_version == '3.8.*'"]
```

---

_Closed by @charliermarsh on 2025-03-30 15:31_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-30 15:31_

---
