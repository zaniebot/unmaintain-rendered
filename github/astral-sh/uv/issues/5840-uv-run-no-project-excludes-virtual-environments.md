---
number: 5840
title: "`uv run --no-project` excludes virtual environments that are not in projects "
type: issue
state: closed
author: zanieb
labels:
  - bug
  - preview
assignees: []
created_at: 2024-08-06T22:38:52Z
updated_at: 2024-08-07T02:52:16Z
url: https://github.com/astral-sh/uv/issues/5840
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv run --no-project` excludes virtual environments that are not in projects 

---

_Issue opened by @zanieb on 2024-08-06 22:38_

e.g. when a virtual environment is active or discovered with a `pyproject.toml` alongside:

```
❯ tree .
.

0 directories, 0 files

❯ uv venv
Using Python 3.12.3 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate

❯ uv run  python -c "import sys; print(sys.executable)"
/Users/zb/workspace/example/.venv/bin/python

❯ uv run --no-project python -c "import sys; print(sys.executable)"
/Users/zb/Library/Caches/uv/builds-v0/.tmphJIdvS/bin/python

❯ source .venv/bin/activate
❯ uv run --no-project python -c "import sys; print(sys.executable)"
/Users/zb/Library/Caches/uv/builds-v0/.tmpxmAgQo/bin/python
```

Should `--no-project` apply here? I'd expect this to require a separate flag.

---

_Comment by @charliermarsh on 2024-08-06 23:05_

Hmm yeah.

---

_Comment by @charliermarsh on 2024-08-06 23:05_

Basically, it should have no effect when you're not in a project (whereas right now, it does).

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-06 23:05_

---

_Label `bug` added by @charliermarsh on 2024-08-06 23:18_

---

_Label `preview` added by @charliermarsh on 2024-08-06 23:18_

---

_Comment by @zanieb on 2024-08-07 01:37_

What would you use to avoid `.venv` detection? `--isolated`?

---

_Comment by @charliermarsh on 2024-08-07 01:38_

I think so?

---

_Referenced in [astral-sh/uv#5846](../../astral-sh/uv/pulls/5846.md) on 2024-08-07 02:05_

---

_Closed by @charliermarsh on 2024-08-07 02:52_

---
