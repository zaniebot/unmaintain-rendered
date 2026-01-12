```yaml
number: 11593
title: Incorrect error message when double open brace is present in f-string
type: issue
state: closed
author: yurivict
labels: []
assignees: []
created_at: 2024-05-29T03:31:42Z
updated_at: 2024-05-29T05:14:00Z
url: https://github.com/astral-sh/ruff/issues/11593
synced_at: 2026-01-12T15:54:51Z
```

# Incorrect error message when double open brace is present in f-string

---

_@yurivict_

This code
```
def fn(a):
    return f'{{a+1}'
```
causes ruff to print
```
$ ruff check x.py
error: Failed to parse x.py:4:19: f-string: single '}' is not allowed
x.py:4:19: E999 SyntaxError: f-string: single '}' is not allowed
Found 1 error.
```

The problem isn't "single '}'", the problem is double '{'.


---

_Comment by @charliermarsh on 2024-05-29 03:33_

Both errors are "correct" though. `f'{{a+1}}'` would also be syntactically valid (it would lead to literal `{` and `}` being printed), and the opening double parenthesis is arguably suggestive that that's correct.

---

_Comment by @dhruvmanila on 2024-05-29 05:11_

Yeah, you can look at it from either perspective and both are correct as Charlie said. This was mainly adopted from the CPython parser:
```
>>> def fn(a):
...     return f'{{a+1}'
  File "<stdin>", line 2
    return f'{{a+1}'
                  ^
SyntaxError: f-string: single '}' is not allowed
```

---

_Comment by @yurivict on 2024-05-29 05:13_

Ok, thanks.

---

_Closed by @yurivict on 2024-05-29 05:13_

---
