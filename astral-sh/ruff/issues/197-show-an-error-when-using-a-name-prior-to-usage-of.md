```yaml
number: 197
title: Show an error when using a name prior to usage of global keyword
type: issue
state: closed
author: Jackenmen
labels: []
assignees: []
created_at: 2022-09-15T02:18:42Z
updated_at: 2022-12-10T16:19:43Z
url: https://github.com/astral-sh/ruff/issues/197
synced_at: 2026-01-10T12:06:12Z
```

# Show an error when using a name prior to usage of global keyword

---

_Issue opened by @Jackenmen on 2022-09-15 02:18_

e.g. this will error at runtime:
```py
TOMATO = "black cherry"


def update_tomato():
    print(TOMATO)  # will error here
    global TOMATO
    TOMATO = "cherry tomato"
```

While this won't:
```py
TOMATO = "black cherry"


def update_tomato():
    global TOMATO
    print(TOMATO)
    TOMATO = "cherry tomato"
```

Seen in pylint:
https://pylint.pycqa.org/en/latest/user_guide/messages/error/used-prior-global-declaration.html

---

_Label `enhancement` added by @charliermarsh on 2022-09-15 02:32_

---

_Comment by @charliermarsh on 2022-11-30 03:13_

This is also [E0118](https://pylint.pycqa.org/en/latest/user_guide/messages/error/used-prior-global-declaration.html) in Pylint.

---

_Comment by @Jackenmen on 2022-12-04 13:34_

I think I may have originally wanted this to be added to `F821` (`UndefinedName`) but I guess it isn't quite the same kind of error + flake8 doesn't report on it:
```
  File "/tmp/blah/test.py", line 6
    global TOMATO
    ^^^^^^^^^^^^^
SyntaxError: name 'TOMATO' is used prior to global declaration
```

---

_Comment by @charliermarsh on 2022-12-09 15:12_

Gonna try to fix this today. (It’ll force me to learn all the rules around globals.)

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-09 17:30_

---

_Comment by @charliermarsh on 2022-12-10 15:02_

Interesting I think these are actually _syntax_ errors?

---

_Closed by @charliermarsh on 2022-12-10 16:19_

---

_Comment by @charliermarsh on 2022-12-10 16:19_

Implemented as `PLE0118` (to match Pylint's `E0118`).

---
