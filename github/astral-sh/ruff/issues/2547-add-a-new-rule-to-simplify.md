---
number: 2547
title: Add a new rule to simplify
type: issue
state: closed
author: JonathanPlasse
labels: []
assignees: []
created_at: 2023-02-03T15:57:33Z
updated_at: 2023-02-03T18:42:47Z
url: https://github.com/astral-sh/ruff/issues/2547
synced_at: 2026-01-07T13:12:14-06:00
---

# Add a new rule to simplify

---

_Issue opened by @JonathanPlasse on 2023-02-03 15:57_

I have this code:
```python
def has_permissions(user: User, permissions: list[PermissionEnum]) -> bool:
    for permission in permissions:
        if permission not in user.permissions:
            return False
    return True
```
which was simplified to:
```python
def has_permissions(self, user: User, permissions: list[PermissionEnum]) -> bool:
    return all(not permission not in user.permissions for permission in permissions)
```
but could be simplified further to:
```python
def has_permissions(self, user: User, permissions: list[PermissionEnum]) -> bool:
    return all(permission in user.permissions for permission in permissions)
```
We could detect when we have `not item not in items` and simplify it to `item in items`.

I used `select = ["ALL"]`.
<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Comment by @charliermarsh on 2023-02-03 16:26_

I think this is a duplicate of #2219 and #2394.

---

_Closed by @charliermarsh on 2023-02-03 16:26_

---

_Comment by @JonathanPlasse on 2023-02-03 18:33_

Sorry for the noise.

---

_Comment by @charliermarsh on 2023-02-03 18:42_

No prob at all.

---
