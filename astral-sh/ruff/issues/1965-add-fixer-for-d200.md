---
number: 1965
title: Add fixer for D200
type: issue
state: closed
author: spaceone
labels:
  - docstring
  - fixes
assignees: []
created_at: 2023-01-18T16:16:35Z
updated_at: 2023-01-20T00:24:51Z
url: https://github.com/astral-sh/ruff/issues/1965
synced_at: 2026-01-10T01:22:40Z
---

# Add fixer for D200

---

_Issue opened by @spaceone on 2023-01-18 16:16_

It would be nice if there was a fixer for `D200` which transforms:
```python
class Foo(object):
    """A wrapper for blah.
        """
```
into
```python
class Foo(object):
    """A wrapper for blah."""
```


---

_Label `docstring` added by @charliermarsh on 2023-01-18 16:41_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-20 00:09_

---

_Label `autofix` added by @charliermarsh on 2023-01-20 00:09_

---

_Referenced in [astral-sh/ruff#2006](../../astral-sh/ruff/pulls/2006.md) on 2023-01-20 00:24_

---

_Closed by @charliermarsh on 2023-01-20 00:24_

---
