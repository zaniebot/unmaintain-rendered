---
number: 4078
title: Broken refactor from UP032
type: issue
state: closed
author: ionelmc
labels:
  - bug
assignees: []
created_at: 2023-04-24T13:47:33Z
updated_at: 2023-04-30T20:57:43Z
url: https://github.com/astral-sh/ruff/issues/4078
synced_at: 2026-01-07T13:12:14-06:00
---

# Broken refactor from UP032

---

_Issue opened by @ionelmc on 2023-04-24 13:47_

This:
```py
a = {'foo': 'bar'}
print('{[foo]}'.format(a))
```
gets changed to:
```py
a = {'foo': 'bar'}
print(f'{a[foo]}')
```

Which obviously is invalid code. Perhaps this type of code should be skipped from UP032?

My ruff settings have `target-version = "py37"`.

---

_Label `bug` added by @charliermarsh on 2023-04-24 15:18_

---

_Comment by @JonathanPlasse on 2023-04-24 17:42_

I would like to work on it.

---

_Comment by @bluetech on 2023-04-24 20:59_

I was curious why the difference here. Answer is here: https://peps.python.org/pep-0498/#differences-between-f-string-and-str-format-expressions. Apparently it is the only difference between f-strings and format!

---

_Comment by @charliermarsh on 2023-04-25 00:02_

Woah, fascinating! Thanks for sharing that, I was wondering the same thing.

---

_Assigned to @JonathanPlasse by @charliermarsh on 2023-04-25 00:02_

---

_Referenced in [astral-sh/ruff#4165](../../astral-sh/ruff/pulls/4165.md) on 2023-04-30 19:40_

---

_Closed by @charliermarsh on 2023-04-30 20:57_

---
