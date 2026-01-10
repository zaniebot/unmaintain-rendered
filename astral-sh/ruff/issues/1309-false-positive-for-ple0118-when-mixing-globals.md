---
number: 1309
title: False positive for PLE0118 when mixing globals and f-strings
type: issue
state: closed
author: PhilReinhold
labels:
  - bug
assignees: []
created_at: 2022-12-21T16:49:28Z
updated_at: 2022-12-21T19:34:10Z
url: https://github.com/astral-sh/ruff/issues/1309
synced_at: 2026-01-10T01:22:39Z
---

# False positive for PLE0118 when mixing globals and f-strings

---

_Issue opened by @PhilReinhold on 2022-12-21 16:49_

It seems like we get a false positive from using globals inside of f-strings

```python
# test.py
x = 1

def f():
    global x
    x = x + 1
    print(f"{x=}")
```

Then

```
$ ruff --version
ruff 0.0.189

$ ruff --select PLE test.py --show-source
Found 1 error(s).
test.py:7:11: PLE0118 Name `x` is used prior to global declaration on line 5
  |
7 |     print(f"{x=}")
  |           ^^^^^^^ PLE0118
  |
```



---

_Comment by @charliermarsh on 2022-12-21 17:04_

Ah yeah, this makes sense. Will fix.

(We have to treat f-strings in a special way everywhere, because RustPython doesn't properly adjust the locations of the nested expressions.)


---

_Label `bug` added by @charliermarsh on 2022-12-21 17:04_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-21 19:23_

---

_Referenced in [astral-sh/ruff#1314](../../astral-sh/ruff/pulls/1314.md) on 2022-12-21 19:32_

---

_Closed by @charliermarsh on 2022-12-21 19:34_

---
