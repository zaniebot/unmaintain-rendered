```yaml
number: 11584
title: Propagate reads on global variables
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/global
created_at: 2024-05-28T18:32:54Z
updated_at: 2024-05-28T18:47:06Z
url: https://github.com/astral-sh/ruff/pull/11584
synced_at: 2026-01-12T15:55:38Z
```

# Propagate reads on global variables

---

_@charliermarsh_

## Summary

This PR ensures that if a variable is bound via `global`, and then the `global` is read, the originating variable is also marked as read. It's not perfect, in that it won't detect _rebindings_, like:

```python
from app import redis_connection

def func():
    global redis_connection

    redis_connection = 1
    redis_connection()
```

So, above, `redis_connection` is still marked as unused.

But it does avoid flagging `redis_connection` as unused in:

```python
from app import redis_connection

def func():
    global redis_connection

    redis_connection()
```

Closes https://github.com/astral-sh/ruff/issues/11518.


---

_Label `bug` added by @charliermarsh on 2024-05-28 18:32_

---

_Comment by @github-actions[bot] on 2024-05-28 18:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-05-28 18:47_

---

_Closed by @charliermarsh on 2024-05-28 18:47_

---

_Branch deleted on 2024-05-28 18:47_

---
