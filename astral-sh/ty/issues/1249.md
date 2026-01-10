```yaml
number: 1249
title: "SQLAlchemy Extensions - Cannot resolve import module `sqlalchemy.ext.asyncio`"
type: issue
state: closed
author: Josh-M42
labels:
  - needs-mre
  - imports
assignees: []
created_at: 2025-09-24T22:12:36Z
updated_at: 2025-09-25T17:51:56Z
url: https://github.com/astral-sh/ty/issues/1249
synced_at: 2026-01-10T02:06:25Z
```

# SQLAlchemy Extensions - Cannot resolve import module `sqlalchemy.ext.asyncio`

---

_Issue opened by @Josh-M42 on 2025-09-24 22:12_

### Summary

Below is the error:

```zsh
error[unresolved-import]: Cannot resolve imported module `sqlalchemy.ext.asyncio`
  --> app/services/proto_conversion.py:20:6
   |
18 | from google.protobuf import struct_pb2
19 | from sqlalchemy import and_, or_, select
20 | from sqlalchemy.ext.asyncio import AsyncSession
   |      ^^^^^^^^^^^^^^^^^^^^^^
21 | from sqlalchemy.orm import outerjoin
   |
info: Searched in the following paths during module resolution:
info:   1. /home/josh/projects/python_cms/backend (first-party code)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   3. /home/josh/projects/python_cms/backend/.venv/lib/python3.13/site-packages (site-packages)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default
```

Below is the ls command showing the ext module is in sqlalchemy, as well as `sqlalchemy.ext.asyncio`.
```zsh
󰣇 projects/python_cms/backend   main  ✘!+? ❯ ls -a .venv/lib/python3.13/site-packages/sqlalchemy/                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      backend 3.13.7  17:11 
.  ..  connectors  cyextension  dialects  engine  event  events.py  exc.py  ext  future  __init__.py  inspection.py  log.py  orm  pool  __pycache__  py.typed  schema.py  sql  testing  types.py  util
󰣇 projects/python_cms/backend   main  ✘!+? ❯ ls -a .venv/lib/python3.13/site-packages/sqlalchemy/ext                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   backend 3.13.7  17:11 
.  ..  associationproxy.py  asyncio  automap.py  baked.py  compiler.py  declarative  horizontal_shard.py  hybrid.py  indexable.py  __init__.py  instrumentation.py  mutable.py  mypy  orderinglist.py  __pycache__  serializer.py
```

### Version

0.0.1-alpha.21

---

_Comment by @carljm on 2025-09-25 00:38_

Thanks for the report!

Are you able to reproduce the issue in a clean project? I'm not able to reproduce it, with this sequence of commands:

```
➜ mkdir sqla
➜ cd sqla
➜ uv init .
Initialized project `sqla` at `/Users/carlmeyer/projects/ruff-examples/sqla`

➜ uv add sqlalchemy
Using CPython 3.13.2
Creating virtual environment at: .venv
Resolved 4 packages in 107ms
Installed 2 packages in 7ms
 + sqlalchemy==2.0.43
 + typing-extensions==4.15.0

➜ echo "from sqlalchemy.ext.asyncio import AsyncSession" > main.py

➜ cat main.py
from sqlalchemy.ext.asyncio import AsyncSession

➜ uvx ty check
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                                                      All checks passed!
```

Without reproduction instructions, it will be hard to track down what's happening here.

---

_Label `imports` added by @carljm on 2025-09-25 00:38_

---

_Label `needs-mre` added by @carljm on 2025-09-25 00:39_

---

_Comment by @Josh-M42 on 2025-09-25 17:51_

@carljm Ah, it seems I'm unable to reproduce it in a clean project. Sorry! I'll try and figure out what the issue is.

---

_Closed by @Josh-M42 on 2025-09-25 17:51_

---
