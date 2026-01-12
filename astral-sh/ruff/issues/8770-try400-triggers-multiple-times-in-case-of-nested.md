```yaml
number: 8770
title: TRY400 triggers multiple times in case of nested try..except blocks
type: issue
state: closed
author: ThiefMaster
labels:
  - bug
assignees: []
created_at: 2023-11-19T20:28:25Z
updated_at: 2023-11-19T23:44:00Z
url: https://github.com/astral-sh/ruff/issues/8770
synced_at: 2026-01-12T15:54:48Z
```

# TRY400 triggers multiple times in case of nested try..except blocks

---

_@ThiefMaster_

```python
try:
    ...
except E1 as exc:
    ...
    try:
        ...
    except E2:
        try:
            ...
        except E3:
            logger.error('foo')
```

```
$ ruff --isolated --select TRY400 --no-cache rufftest/ruff_sample.py
rufftest/ruff_sample.py:11:13: TRY400 Use `logging.exception` instead of `logging.error`
rufftest/ruff_sample.py:11:13: TRY400 Use `logging.exception` instead of `logging.error`
rufftest/ruff_sample.py:11:13: TRY400 Use `logging.exception` instead of `logging.error`
Found 3 errors.
```

---

_Label `bug` added by @charliermarsh on 2023-11-19 22:58_

---

_Closed by @charliermarsh on 2023-11-19 23:44_

---
