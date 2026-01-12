```yaml
number: 4522
title: "[Autofix error] UP036 autofix fails on complex nested block"
type: issue
state: closed
author: konstin
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-05-19T08:20:45Z
updated_at: 2023-05-28T03:37:24Z
url: https://github.com/astral-sh/ruff/issues/4522
synced_at: 2026-01-12T15:54:44Z
```

# [Autofix error] UP036 autofix fails on complex nested block

---

_@konstin_

UP036 transforms the following 

```python
import sys

if sys.version_info < (3, 8):

    def a():
        if b:
            print(1)
        elif c:
            print(2)
        return None

else:
    pass
```

to 

```python
import sys

if c:
            print(2)
        return None

else:
    pass
```

which is invalid syntax and also removed ~~a lot of~~ the code incompletely.

Command:

```
ruff --no-cache --select UP036 --fix code.py 
```

ruff version fd16d658e9407ce76bc38cd8f5112dd272fc5be0


---

_Label `autofix` added by @konstin on 2023-05-19 08:20_

---

_Comment by @charliermarsh on 2023-05-19 12:33_

Removing code is intended (it's removing the outdated version check based on the currently-specified minimum version), but the output is clearly wrong.

---

_Label `bug` added by @charliermarsh on 2023-05-19 12:33_

---

_Label `autofix-error` added by @charliermarsh on 2023-05-19 12:33_

---

_Comment by @JonathanPlasse on 2023-05-19 19:36_

I would like to work on this.

---

_Assigned to @JonathanPlasse by @charliermarsh on 2023-05-19 19:36_

---

_Comment by @charliermarsh on 2023-05-19 19:36_

Thanks @JonathanPlasse.

---

_Closed by @charliermarsh on 2023-05-28 03:37_

---
