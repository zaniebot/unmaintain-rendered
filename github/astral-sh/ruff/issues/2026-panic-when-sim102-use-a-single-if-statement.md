---
number: 2026
title: "[Panic] when SIM102 Use a single `if` statement instead of nested `if` statements"
type: issue
state: closed
author: ded42r
labels:
  - bug
assignees: []
created_at: 2023-01-20T12:52:42Z
updated_at: 2023-01-20T14:08:06Z
url: https://github.com/astral-sh/ruff/issues/2026
synced_at: 2026-01-07T13:12:14-06:00
---

# [Panic] when SIM102 Use a single `if` statement instead of nested `if` statements

---

_Issue opened by @ded42r on 2023-01-20 12:52_

I ran the command  
> ruff --isolated --fix --select SIM .\min_example.py

and got an error.
```
error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/charliermarsh/ruff/issues/new?title=%5BPanic%5D

quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', src\rules\flake8_simplify\rules\fix_if.rs:124:9
stack backtrace:
   0:     0x7ff7c0982d15 - <unknown>
   1:     0x7ff7c0410f4b - <unknown>
   2:     0x7ff7c097aa91 - <unknown>
   3:     0x7ff7c0985ec2 - <unknown>
   4:     0x7ff7c0985b0d - <unknown>
   5:     0x7ff7c06737c7 - <unknown>
   6:     0x7ff7c0986830 - <unknown>
   7:     0x7ff7c0986583 - <unknown>
   8:     0x7ff7c09842ef - <unknown>
   9:     0x7ff7c0986264 - <unknown>
  10:     0x7ff7c0a2e1b5 - <unknown>
  11:     0x7ff7c0a2e05c - <unknown>
  12:     0x7ff7c07a93f8 - <unknown>
  13:     0x7ff7c071ff53 - <unknown>
  14:     0x7ff7c073cbf2 - <unknown>
  15:     0x7ff7c07555df - <unknown>
  16:     0x7ff7c0765dd8 - <unknown>
  17:     0x7ff7c0771ad2 - <unknown>
  18:     0x7ff7c06647cb - <unknown>
  19:     0x7ff7c060495f - <unknown>
  20:     0x7ff7c065c2dc - <unknown>
  21:     0x7ff7c0672c8a - <unknown>
  22:     0x7ff7c05e18e6 - <unknown>
  23:     0x7ff7c05e1d66 - <unknown>
  24:     0x7ff7c097376d - <unknown>
  25:     0x7ff7c068bfcc - <unknown>
  26:                0x2 - <unknown>
  27:     0x7ff7c0a2c401 - <unknown>
  28:     0x7ff7c0a2c10c - <unknown>
  29:     0x7ff9dcc37034 - BaseThreadInitThunk
  30:     0x7ff9de7426a1 - RtlUserThreadStart
```


**environment**: ruff version 0.0.227, python 3.8.10, windows 10

The content of _min_example.py_ are
```python
class A:
    def get_one_or_another(self, first_option: bool, second_option: bool) -> str:
        if first_option:
            if second_option:
                return "one"
        return "other"


if __name__ == "__main__":
    print(A().get_one_or_another(True, True))
```


---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-20 13:11_

---

_Comment by @charliermarsh on 2023-01-20 13:11_

Taking a look, thanks :)

---

_Label `bug` added by @charliermarsh on 2023-01-20 13:12_

---

_Referenced in [astral-sh/ruff#2028](../../astral-sh/ruff/pulls/2028.md) on 2023-01-20 13:36_

---

_Closed by @charliermarsh on 2023-01-20 14:08_

---

_Referenced in [astral-sh/ruff#2033](../../astral-sh/ruff/pulls/2033.md) on 2023-01-20 16:36_

---
