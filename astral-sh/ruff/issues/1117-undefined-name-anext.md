```yaml
number: 1117
title: "Undefined name `anext`"
type: issue
state: closed
author: rgilton
labels: []
assignees: []
created_at: 2022-12-07T12:02:15Z
updated_at: 2022-12-07T15:38:07Z
url: https://github.com/astral-sh/ruff/issues/1117
synced_at: 2026-01-12T15:54:41Z
```

# Undefined name `anext`

---

_@rgilton_

Running ruff 0.0.166 against this file:
```py
print(anext)
```
results in this error:
```
Found 1 error(s).
test.py:2:7: F821 Undefined name `anext`
```
`anext` is a [builtin in python 3.10](https://docs.python.org/3.10/library/functions.html?highlight=anext#anext), so I _think_ it should be supported by ruff?


---

_Comment by @JonathanPlasse on 2022-12-07 12:14_

Will fix.

---

_Closed by @charliermarsh on 2022-12-07 14:20_

---

_Comment by @charliermarsh on 2022-12-07 15:38_

This is going out in [v0.0.167](https://github.com/charliermarsh/ruff/releases/tag/v0.0.167) (building now).

---
