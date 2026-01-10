```yaml
number: 119
title: "`F821 Undefined name` when definition is after usage"
type: issue
state: closed
author: samuelcolvin
labels:
  - bug
assignees: []
created_at: 2022-09-07T12:31:02Z
updated_at: 2022-09-08T02:34:44Z
url: https://github.com/astral-sh/ruff/issues/119
synced_at: 2026-01-10T15:56:05Z
```

# `F821 Undefined name` when definition is after usage

---

_Issue opened by @samuelcolvin on 2022-09-07 12:31_

Ruff looks great, congratulations. I'd love to use it one day on pydantic instead of flake8.

---

The following code is valid python and runs fine

```py
def main():
    foo()

def foo():
    print('this is foo')

main()
```

However ruff returns:

```
test.py:2:5: F821 Undefined name `foo`

Found 1 error(s).
```

Running ruff on pydantic's code base is currently returning 151 `F821` errors, so it's a fairly common problem.


---

_Comment by @charliermarsh on 2022-09-07 13:19_

Thank you! Really flattered by the interest.

This is a legit bug. We have to defer checking function scopes until we've parsed the rest of the module. It's known but unhandled. Gonna use this issue for tracking.


---

_Comment by @samuelcolvin on 2022-09-07 13:20_

great, thanks so much.

---

_Label `bug` added by @charliermarsh on 2022-09-07 14:14_

---

_Closed by @charliermarsh on 2022-09-08 02:34_

---
