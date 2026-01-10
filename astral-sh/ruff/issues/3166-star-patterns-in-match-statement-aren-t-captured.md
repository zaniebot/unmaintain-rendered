```yaml
number: 3166
title: "Star patterns in `match` statement aren't captured"
type: issue
state: closed
author: dosisod
labels:
  - bug
assignees: []
created_at: 2023-02-23T07:29:31Z
updated_at: 2023-02-23T12:39:05Z
url: https://github.com/astral-sh/ruff/issues/3166
synced_at: 2026-01-10T11:09:46Z
```

# Star patterns in `match` statement aren't captured

---

_Issue opened by @dosisod on 2023-02-23 07:29_

The following program:

```python
match [123]:
    case [*x]:
        print(x)

match {"abc": 123}:
    case {**x}:
        print(x)
```

Emits the following errors:

```
$ ruff file.py
file.py:3:15: F821 Undefined name `x`
file.py:7:15: F821 Undefined name `x`
```

Currently using the latest version:

```
$ ruff --version
ruff 0.0.252
```

---

_Label `bug` added by @charliermarsh on 2023-02-23 12:25_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-23 12:25_

---

_Comment by @charliermarsh on 2023-02-23 12:25_

Good catch, thank you.

---

_Closed by @charliermarsh on 2023-02-23 12:39_

---
