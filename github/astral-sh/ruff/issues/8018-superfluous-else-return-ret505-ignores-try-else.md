---
number: 8018
title: "`superfluous-else-return` (RET505) ignores try/else blocks"
type: issue
state: open
author: korverdev
labels:
  - rule
assignees: []
created_at: 2023-10-17T15:29:08Z
updated_at: 2025-06-14T20:04:08Z
url: https://github.com/astral-sh/ruff/issues/8018
synced_at: 2026-01-07T13:12:15-06:00
---

# `superfluous-else-return` (RET505) ignores try/else blocks

---

_Issue opened by @korverdev on 2023-10-17 15:29_

This is probably harder to check for and much more uncommon, but RET505 could potentially apply to `try/else` blocks as well.

Example of the admittedly pretty bad code where this cropped up for me:
```python
try:
    run("docker --version", check=True, shell=True)
except CalledProcessError:
    return "podman"
else:
    return "docker"
```

What I'd expect this to be changed to:
```python
try:
    run("docker --version", check=True, shell=True)
except CalledProcessError:
    return "podman"
return "docker"
```

---

_Label `rule` added by @zanieb on 2023-10-17 17:49_

---

_Label `needs-decision` added by @zanieb on 2023-10-17 17:49_

---

_Comment by @tdulcet on 2024-06-09 14:37_

#970 says that [Pylint R1705](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/no-else-return.html) is implemented by `RET505`, but Pylint does supports else blocks on try statements, while this rule does not.

Also see #9625.

---

_Label `needs-decision` removed by @charliermarsh on 2024-06-09 17:27_

---

_Comment by @charliermarsh on 2024-06-09 17:28_

Seems reasonable to support.

---

_Comment by @TomMontagnon on 2025-06-13 14:26_

It doesn't manage `elif` statements well too.

This `elif` raise `RET505`, but is clearly not superflous.

```
def my_fonc():
    While True
        resp = input("y/n question : ")
        if resp == "y":
            foo()
            return True
        elif resp == "n":
            bar()
            return False
        else:
            print("try again")
```

---

_Comment by @tdulcet on 2025-06-14 19:57_

> This `elif` raise `RET505`, but is clearly not superfluous.

Both the `elif` and `else` in your example look superfluous to me:
```py
def my_fonc():
    while True:
        resp = input("y/n question : ")
        if resp == "y":
            foo()
            return True

        if resp == "n":
            bar()
            return False

        print("try again")
```

---

_Comment by @TomMontagnon on 2025-06-14 20:04_

Yup, I hadn't seen things that way.... you are perfectly right, thanks !

---
