```yaml
number: 2377
title: Match-case causes syntax errors
type: issue
state: closed
author: nefrob
labels: []
assignees: []
created_at: 2023-01-31T05:32:51Z
updated_at: 2023-02-22T00:05:02Z
url: https://github.com/astral-sh/ruff/issues/2377
synced_at: 2026-01-10T11:09:45Z
```

# Match-case causes syntax errors

---

_Issue opened by @nefrob on 2023-01-31 05:32_

Ex.

```python
x = 1
match x:
    case 1:
        pass

E999 SyntaxError: invalid syntax. Got unexpected token 'x'
```

when running `ruff --fix` on `v0.0.238` and with `target-version = "py311"` in `pyproject.toml`.


---

_Comment by @messense on 2023-01-31 05:59_

Duplicate of https://github.com/charliermarsh/ruff/issues/282

---

_Comment by @charliermarsh on 2023-01-31 12:21_

Just closing as duplicate.

---

_Closed by @charliermarsh on 2023-01-31 12:21_

---

_Comment by @charliermarsh on 2023-02-22 00:05_

Fixed as of v0.0.250.

---
