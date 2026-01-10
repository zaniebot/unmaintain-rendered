```yaml
number: 2632
title: UP003 false-positive introduces bug
type: issue
state: closed
author: spaceone
labels:
  - question
assignees: []
created_at: 2023-02-07T19:00:36Z
updated_at: 2023-02-08T17:18:38Z
url: https://github.com/astral-sh/ruff/issues/2632
synced_at: 2026-01-10T11:09:45Z
```

# UP003 false-positive introduces bug

---

_Issue opened by @spaceone on 2023-02-07 19:00_

```python
arg = 1   # any type, not string
type(arg)(' ') in arg
```

`ruff --fix --select UP003 foo.py` turns it into:

```python
arg = 1   # any type, not string
str in arg
```

---

_Renamed from "UP003 introduces bug" to "UP003 false-positive introduces bug" by @spaceone on 2023-02-07 19:01_

---

_Comment by @charliermarsh on 2023-02-07 19:56_

Can you explain what this code is doing? Heh I'm having a hard time understanding it.

---

_Label `question` added by @charliermarsh on 2023-02-07 19:56_

---

_Comment by @spaceone on 2023-02-08 08:26_

It's pseudocode, simplified from somewhere else where `arg` is not a `str`.
but that's irrelevant: 
`UP003` should fix `type(' ')` but not `type(something)(' ')` when it doesn't inspect what `something` is.

---

_Closed by @charliermarsh on 2023-02-08 17:18_

---
