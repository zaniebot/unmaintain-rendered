---
number: 21000
title: ruff suggests invalid code when fixing UP032
type: issue
state: closed
author: dima179-fuse
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-10-20T16:49:21Z
updated_at: 2025-10-20T23:34:04Z
url: https://github.com/astral-sh/ruff/issues/21000
synced_at: 2026-01-07T13:12:16-06:00
---

# ruff suggests invalid code when fixing UP032

---

_Issue opened by @dima179-fuse on 2025-10-20 16:49_

### Summary

Consider the following code:
```python
if __name__ == "__main__":
    number = 0
    string = "{}".format(number := number + 1)
    print(string)
```

It executes successfully and prints `1`. 

When I run ruff, it returns
```
bad_ruff.py:3:14: UP032 [*] Use f-string instead of `format` call
  |
1 | if __name__ == "__main__":
2 |     number = 0
3 |     string = "{}".format(number := number + 1)
  |              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ UP032
4 |     print(string)
  |
  = help: Convert to f-string
```

When I run `ruff check --fix bad_ruff.py`, the resulting code is invalid and results in `ValueError`:

```python
if __name__ == "__main__":
    number = 0
    string = f"{number := number + 1}"
    print(string)

```

### Version

ruff 0.11.13 (5faf72a4d 2025-06-05)

---

_Comment by @dima179-fuse on 2025-10-20 16:50_

Checked and the issue also happens on `ruff 0.14.1 (2bffef596 2025-10-16)`

---

_Comment by @ntBre on 2025-10-20 17:44_

Thanks for the report! Looks like we can't just insert the assignment expression straight into the f-string. Parenthesizing it should be enough to fix the error:

```pycon
>>> number = 0
>>> f"{number := number + 1}"
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    f"{number := number + 1}"
      ^^^^^^^^^^^^^^^^^^^^^^
ValueError: Invalid format specifier '= number + 1' for object of type 'int'
>>> f"{(number := number + 1)}"
'1'
```

---

_Label `bug` added by @ntBre on 2025-10-20 17:44_

---

_Label `fixes` added by @ntBre on 2025-10-20 17:44_

---

_Referenced in [astral-sh/ruff#21003](../../astral-sh/ruff/pulls/21003.md) on 2025-10-20 18:16_

---

_Closed by @dylwil3 on 2025-10-20 23:34_

---
