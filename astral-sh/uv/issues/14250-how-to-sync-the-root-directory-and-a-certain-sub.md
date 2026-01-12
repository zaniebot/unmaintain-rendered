```yaml
number: 14250
title: How to sync the root directory and a certain sub-project dependencies
type: issue
state: closed
author: vvanglro
labels:
  - question
assignees: []
created_at: 2025-06-25T03:01:06Z
updated_at: 2025-06-26T01:34:15Z
url: https://github.com/astral-sh/uv/issues/14250
synced_at: 2026-01-12T16:01:45Z
```

# How to sync the root directory and a certain sub-project dependencies

---

_@vvanglro_

### Question

```tree
albatross
├── packages
│   ├── bird-feeder
│   │   ├── pyproject.toml
│   │   └── src
│   │       └── bird_feeder
│   │           ├── __init__.py
│   │           └── foo.py
│   └── seeds
│       ├── pyproject.toml
│       └── src
│           └── seeds
│               ├── __init__.py
│               └── bar.py
├── pyproject.toml
├── README.md
├── uv.lock
└── src
    └── albatross
        └── main.py
```
I executed `uv sync --package seeds` in the root directory, which would also simultaneously uninstall dependencies in the root directory.

If I only want to synchronize the `root` directory and the `seeds` subproject dependencies, how should I do it?



### Platform

_No response_

### Version

uv 0.7.14 (Homebrew 2025-06-23)

---

_Label `question` added by @vvanglro on 2025-06-25 03:01_

---

_Comment by @oconnor663 on 2025-06-25 20:28_

Have you looked at `uv sync --inexact` ?

---

_Comment by @vvanglro on 2025-06-26 01:16_

> Have you looked at `uv sync --inexact` ?

This is useful to me, thank you.

Need to execute two commands, the first one is to run `uv sync` under the root project, and the second one is to select a subproject where you need to install dependencies and run `uv sync --inexact --package seeds`.

---

_Closed by @vvanglro on 2025-06-26 01:16_

---
