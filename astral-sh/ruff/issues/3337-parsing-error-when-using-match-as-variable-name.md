```yaml
number: 3337
title: "Parsing error when using `match` as variable name"
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-03-04T14:04:37Z
updated_at: 2023-03-04T17:42:07Z
url: https://github.com/astral-sh/ruff/issues/3337
synced_at: 2026-01-10T11:09:46Z
```

# Parsing error when using `match` as variable name

---

_Issue opened by @qarmin on 2023-03-04 14:04_

```
event = 12
match = lambda query: query == event
print(match(12))
print(match(2))
```
prints with python 3.10 True, False, but ruff not wants to parse it due 
```
error: Failed to parse aa.py: invalid syntax. Got unexpected token '=' at line 2 column 7
aa.py:2:8: E999 SyntaxError: invalid syntax. Got unexpected token '='
```


Ruff 0.0.254
Config 
```
select = ["A", "B", "C", "E", "F"]

exclude = [
]

line-length = 140
```


---

_Comment by @qarmin on 2023-03-04 14:07_

Similar problems with other builtin names 
```
Lookup.async = None
```
cause
```
invalid syntax. Got unexpected token 'async' at line 43 column 10
```
or 
```
case = lambda re, **kwargs: (re, TypeError, kwargs)
```

```
invalid syntax. Got unexpected token 'case' at line 485 column 5
```

---

_Label `bug` added by @charliermarsh on 2023-03-04 15:01_

---

_Comment by @charliermarsh on 2023-03-04 15:02_

Thanks, that’s surprising, I’ll take a look!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-04 16:07_

---

_Comment by @charliermarsh on 2023-03-04 16:50_

Hmm, I don't think `Lookup.async = None` is valid Python. `async` and `await` became reserved keywords in Python 3.7.

The main issue here is the handling of `match` and `case` with lambda assignments.

---

_Comment by @charliermarsh on 2023-03-04 17:09_

Fixed by https://github.com/RustPython/RustPython/pull/4623 in RustPython.

---

_Closed by @charliermarsh on 2023-03-04 17:42_

---
