---
number: 5678
title: Nested bare except not found
type: issue
state: closed
author: puittenbroek
labels: []
assignees: []
created_at: 2023-07-11T08:24:09Z
updated_at: 2023-07-11T13:26:50Z
url: https://github.com/astral-sh/ruff/issues/5678
synced_at: 2026-01-07T13:12:15-06:00
---

# Nested bare except not found

---

_Issue opened by @puittenbroek on 2023-07-11 08:24_

Tried to search for an existing issue regarding this but couldn't find any. Apologies if my search was insufficient.
Busy introducing ruff for our repositories and ran into situation where it didn't find all issues that flake8 did raise.
Most notably this 'bare except' issue. 

A minimal code snippet that reproduces the bug.:
```python
def some_function(*args, **kwargs):
    try:
        1 / 0
    except:
        raise


if __name__ == "__main__":
    try:
        1 / 0
    except:
        pass

    some_function()

```
The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.:
```bash
ruff check --isolated -n broken.py
```

The current Ruff version (`ruff --version`):
```
ruff 0.0.277
```

Output:
```
broken.py:11:5: E722 Do not use bare `except`
Found 1 error.
```

Expected it to also find line 4, flake8 does find this; `flake8 broken.py`:
```
broken.py:4:5: E722 do not use bare 'except'
broken.py:11:5: E722 do not use bare 'except'

```

---

_Comment by @charliermarsh on 2023-07-11 13:16_

I believe it's because you're re-raising the exception on line 5, which we allow, since it then forces the exception to be handled by the caller.

---

_Comment by @puittenbroek on 2023-07-11 13:20_

Ha.. you're right. If I replace it with a `pass` ruff does error on it. Case closed then :+1: 

---

_Closed by @puittenbroek on 2023-07-11 13:20_

---

_Label `waiting-on-author` added by @charliermarsh on 2023-07-11 13:26_

---

_Comment by @charliermarsh on 2023-07-11 13:26_

Thanks for the clear issue :)

---

_Label `waiting-on-author` removed by @charliermarsh on 2023-07-11 13:26_

---
