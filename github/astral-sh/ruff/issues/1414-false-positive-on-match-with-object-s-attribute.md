---
number: 1414
title: "False positive on match with object's attribute"
type: issue
state: closed
author: Akomer
labels: []
assignees: []
created_at: 2022-12-27T23:43:22Z
updated_at: 2023-02-22T00:05:38Z
url: https://github.com/astral-sh/ruff/issues/1414
synced_at: 2026-01-07T13:12:14-06:00
---

# False positive on match with object's attribute

---

_Issue opened by @Akomer on 2022-12-27 23:43_

I got a false positive SyntaxError in case i am using the match on an object's attribute:

```python
class A:
    var1 = 5


def false_positive_case():
    a = A()
    match a.var1:
        case 5:
            print('easy match')
        case _:
            print('sad')


def okay_case():
    a = [1, 2, 3]
    match a:
        case [v1, v2, *_]:
            print('easy matcH:', v1, v2)


if __name__ == '__main__':
    false_positive_case()
    okay_case()
```

```shell
ruff ruff_match.py
ruff_match.py:7:12: E999 SyntaxError: invalid syntax. Got unexpected token 'a'
Found 1 error(s).
```

versions:
Python 3.10.6
ruff 0.0.196

---

_Comment by @charliermarsh on 2022-12-28 00:42_

Sadly, we don't yet support structural pattern matching (see: #282). It's the one language feature that we're still working on. 

---

_Closed by @charliermarsh on 2022-12-28 00:42_

---

_Comment by @charliermarsh on 2023-02-22 00:05_

Fixed as of v0.0.250 which included `match` statement support.

---
