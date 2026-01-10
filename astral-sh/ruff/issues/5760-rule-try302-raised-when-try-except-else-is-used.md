```yaml
number: 5760
title: Rule TRY302 raised when try-except-else is used
type: issue
state: closed
author: cbenz
labels: []
assignees: []
created_at: 2023-07-14T10:35:11Z
updated_at: 2023-07-14T11:42:56Z
url: https://github.com/astral-sh/ruff/issues/5760
synced_at: 2026-01-10T11:09:48Z
```

# Rule TRY302 raised when try-except-else is used

---

_Issue opened by @cbenz on 2023-07-14 10:35_

The following code raises `TRY302` :

```python
try:
    f()
except Exception:
    raise
else:
    ...
```

However the following code is a Python syntax error:

```python
try:
    f()
else:
    ...
```

I think that in this case the error `TRY302` should not occur.

---

_Comment by @cbenz on 2023-07-14 11:42_

I think I'm completely mistaken. This should be done:

```python
f()
...
```

Sorry for the noise.

---

_Closed by @cbenz on 2023-07-14 11:42_

---
