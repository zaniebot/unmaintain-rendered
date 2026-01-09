---
number: 9638
title: "New rule: find unused placeholders for f-strings"
type: issue
state: closed
author: Sanchoyzer
labels: []
assignees: []
created_at: 2024-01-24T19:55:05Z
updated_at: 2024-01-24T20:25:01Z
url: https://github.com/astral-sh/ruff/issues/9638
synced_at: 2026-01-07T13:12:15-06:00
---

# New rule: find unused placeholders for f-strings

---

_Issue opened by @Sanchoyzer on 2024-01-24 19:55_

`ruff==0.1.14`
```
def func(my_var: int) -> int:
    var = my_var * 2
    print('var is {var}')
    return var
```
It might be useful if Ruff will find such places, I made a typo: `'var is {var}'` instead of `f'var is {var}'`


---

_Comment by @trag1c on 2024-01-24 20:23_

I think this is a duplicate of #8151

---

_Comment by @charliermarsh on 2024-01-24 20:25_

Agreed -- I'd like to add this!

---

_Closed by @charliermarsh on 2024-01-24 20:25_

---
