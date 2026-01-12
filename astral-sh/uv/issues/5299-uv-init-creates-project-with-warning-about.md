```yaml
number: 5299
title: "`uv init` creates project with warning about `Requires-Python`"
type: issue
state: closed
author: zanieb
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-22T17:59:36Z
updated_at: 2024-07-23T16:02:41Z
url: https://github.com/astral-sh/uv/issues/5299
synced_at: 2026-01-12T15:58:55Z
```

# `uv init` creates project with warning about `Requires-Python`

---

_@zanieb_

```
❯ mkdir example
❯ cd example
❯ uv init
warning: `uv init` is experimental and may change without warning
Initialized project example
❯ tree .
.
├── README.md
├── pyproject.toml
└── src
    └── example
        └── __init__.py

3 directories, 3 files
❯ uv run -- python -c "import example"
warning: `uv run` is experimental and may change without warning
Using Python 3.12.1 interpreter at: /Users/zb/Library/Application Support/uv/python/cpython-3.12.1-macos-aarch64-none/bin/python3
Creating virtualenv at: .venv
warning: No `requires-python` field found in the workspace. Defaulting to `>=3.12`.
Resolved 1 package in 2ms
   Built example @ file:///Users/zb/workspace/example
Prepared 1 package in 1.02s
Installed 1 package in 0.86ms
 + example==0.1.0 (from file:///Users/zb/workspace/example)
```

We should set a Python requirement to avoid creating warnings in our own tool:

> warning: No `requires-python` field found in the workspace. Defaulting to `>=3.12`.

---

_Label `bug` added by @charliermarsh on 2024-07-22 18:41_

---

_Label `preview` added by @charliermarsh on 2024-07-22 18:41_

---

_Comment by @charliermarsh on 2024-07-22 18:41_

Is it just: the version of the current interpreter?

---

_Comment by @zanieb on 2024-07-22 18:45_

Probably. Or our "default" (the latest) Python version if that's not present?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-22 21:23_

---

_Closed by @charliermarsh on 2024-07-23 16:02_

---

_Closed by @charliermarsh on 2024-07-23 16:02_

---
