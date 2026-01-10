---
number: 16147
title: "[Question] Resolving imports in IDE with uv for `package=False`"
type: issue
state: closed
author: vaibhavmano
labels:
  - question
assignees: []
created_at: 2025-10-07T07:13:20Z
updated_at: 2025-10-07T09:23:28Z
url: https://github.com/astral-sh/uv/issues/16147
synced_at: 2026-01-10T01:26:03Z
---

# [Question] Resolving imports in IDE with uv for `package=False`

---

_Issue opened by @vaibhavmano on 2025-10-07 07:13_

### Question

I'm in the process of migrating all my services to uv. We follow a monorepo in my org. While I haven't explicitly followed the src folder structure, I have managed to use various uv settings to get it to work. I'm now facing an issue with the IDE (Pycharm) not being able to recognize local imports.

```
[project]
name = "batchuser"
version = "1.0.0"
description = "batch user service"
authors = [{ email = "" }]
requires-python = "==3.12.*"
dependencies = [
    "baselib==0.1.0",
    "scim2-filter-parser==0.7.0"
]

[tool.uv]
package = false

[tool.uv.sources]
batchuser = { path = "." }
baselib = { workspace = true }


[dependency-groups]
dev = [
    "ruff==0.13.0",
    "bandit>=1.8.6",
    "pytest>=8.4.2",
    "pytest-cov>=7.0.0",
]

[tool.pytest.ini_options]
pythonpath = ["."]
addopts = "--cov=. --cov-report=xml"
testpaths = ["test"]

```

This is the `pyproject.toml` for one of the service which doesn't follow src folder structure. 

```
├── uv.lock
├── pyproject.toml
├── baselib/
│       ├── pyproject.toml
│       ├── Dockerfile
│       ├── base/
│       │     ├── constants
│       │     ├── common
│       │     └── ..
│       .
│
│
├── batchuser/
│   │   ├── __init__.py
│   │   ├── pyproject.toml
│   │   ├── Dockerfile
│   .   ├── constants/
│       ├── services/
│       ├── server/
│       └── ..
.
```

Inside the `batchuser/service/app_service.py`,
```
from base.server.db.doc_session import DocumentSession. # Works perfectly fine since baselib is built as a package
from base.util import validate_email

from constant import RequestConstants, SyncConstants # Throws error in IDE for import
```
I have to mention though that the `uv run` and `pytest` commands work perfectly fine when triggered from the terminal. I understand this is expected since `batchuser` is not built as a package but I intent to keep it that way. 

This is only an issue during development since I won't be able to (CMD+click) to navigate to the import or find usages. Should I convert the folder to src structure even though it's not a package?

### Platform

macOS 14 intel

### Version

uv 0.8.17

---

_Label `question` added by @vaibhavmano on 2025-10-07 07:13_

---

_Closed by @vaibhavmano on 2025-10-07 09:23_

---
