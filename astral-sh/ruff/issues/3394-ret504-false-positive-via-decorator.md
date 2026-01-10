---
number: 3394
title: RET504 false positive via decorator
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-03-08T00:31:29Z
updated_at: 2023-03-08T00:38:03Z
url: https://github.com/astral-sh/ruff/issues/3394
synced_at: 2026-01-10T01:22:42Z
---

# RET504 false positive via decorator

---

_Issue opened by @charliermarsh on 2023-03-08 00:31_

Here's an interesting case where variable is assigned to, referenced, and then returned:
https://github.com/KyleKing/pytest_cache_assert/blob/47d0b538528a9c9cdbc182d2b276462a2aa0fd4d/tests/test_flask.py#L20
```python
def _create_app() -> Flask:
    app = Flask(__name__)

    @app.route('/hello')
    def hello() -> str:
        """Hello endpoint."""
        return 'Hello, World!'

    return app  # noqa: RET504
```
Seems like something that shouldn't be flagged.

Perhaps instead of an assignment count, heuristic could be reference count. In other words, excluding return statement, flag if the only reference was assignment.

_Originally posted by @gdub in https://github.com/charliermarsh/ruff/issues/2950#issuecomment-1456740839_
            

---

_Label `bug` added by @charliermarsh on 2023-03-08 00:31_

---

_Referenced in [astral-sh/ruff#3395](../../astral-sh/ruff/pulls/3395.md) on 2023-03-08 00:31_

---

_Closed by @charliermarsh on 2023-03-08 00:38_

---
