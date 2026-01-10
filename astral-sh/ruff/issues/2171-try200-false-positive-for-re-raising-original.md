```yaml
number: 2171
title: TRY200 false positive for re-raising original/currently handled exception
type: issue
state: closed
author: scop
labels:
  - bug
assignees: []
created_at: 2023-01-25T20:17:07Z
updated_at: 2023-01-25T21:23:35Z
url: https://github.com/astral-sh/ruff/issues/2171
synced_at: 2026-01-10T11:09:45Z
```

# TRY200 false positive for re-raising original/currently handled exception

---

_Issue opened by @scop on 2023-01-25 20:17_

ruff 0.0.233 issues TRY200 for the following cases:

```python
try:
    raise RuntimeError
except Exception:
    # ...do something...
    raise
```

```python
try:
    raise RuntimeError
except Exception as ex:
    # ...do something...
    raise ex
```

I don't think TRY200 should be issued when re-raising the exception currently being handled (plain `raise`) or when the original exception is being explicitly re-raised.

---

_Comment by @charliermarsh on 2023-01-25 20:23_

Yeah that seems like an error to me.

---

_Label `bug` added by @charliermarsh on 2023-01-25 20:23_

---

_Comment by @charliermarsh on 2023-01-25 20:31_

Tryceratops doesn't flag that, so definitely a bug.

---

_Comment by @charliermarsh on 2023-01-25 20:38_

I'll fix this real quick, so that we avoid ever releasing this version. (Are you building from source? :))

---

_Closed by @charliermarsh on 2023-01-25 20:51_

---

_Comment by @scop on 2023-01-25 21:23_

Thanks! Not building from source, looks like I managed to grab 0.0.233 before it was yanked.

---
