---
number: 6931
title: "`no-indented-block` (`E112`) rule is not needed because it's already covered by `syntax-error` (`E999`)"
type: issue
state: closed
author: DetachHead
labels:
  - needs-decision
assignees: []
created_at: 2023-08-28T07:08:17Z
updated_at: 2024-01-12T23:26:38Z
url: https://github.com/astral-sh/ruff/issues/6931
synced_at: 2026-01-07T13:12:15-06:00
---

# `no-indented-block` (`E112`) rule is not needed because it's already covered by `syntax-error` (`E999`)

---

_Issue opened by @DetachHead on 2023-08-28 07:08_

```py
for item in items:
pass
```
```
main.py:2:1: E112 Expected an indented block
main.py:2:1: E999 SyntaxError: Expected 'Indent', but got 'pass'
```

since `E112` is a nursery rule i think it should just be removed, especially since there are false positives (eg. #6930)

---

_Label `needs-decision` added by @charliermarsh on 2023-08-29 16:51_

---

_Comment by @charliermarsh on 2024-01-10 21:17_

I think I'm okay with this because, while rare, it can _also_ catch cases that _aren't_ syntax errors, like:

```python
def print_list(my_list):
# Some comment
    print('Nope')
```

---

_Closed by @charliermarsh on 2024-01-10 21:17_

---

_Comment by @DetachHead on 2024-01-12 23:26_

i know black covers that case, looks like `ruff format` does too https://play.ruff.rs/36beb564-71e4-4e1e-a402-e67569688a97?secondary=Format

---
