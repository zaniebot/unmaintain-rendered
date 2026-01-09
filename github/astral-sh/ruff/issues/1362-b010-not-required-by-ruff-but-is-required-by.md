---
number: 1362
title: B010 not required by ruff, but is required by flake8 bugbear.
type: issue
state: closed
author: ghuls
labels:
  - bug
assignees: []
created_at: 2022-12-24T18:17:26Z
updated_at: 2022-12-24T19:09:35Z
url: https://github.com/astral-sh/ruff/issues/1362
synced_at: 2026-01-07T13:12:14-06:00
---

# B010 not required by ruff, but is required by flake8 bugbear.

---

_Issue opened by @ghuls on 2022-12-24 18:17_

```
# If # noqa: B010 is not set.
$ flake8 test_B010.py
test_B010.py:7:5: B010 Do not call setattr with a constant attribute value, it is not any safer than normal property access.

$ ruff test_B010.py

$ ruff --extend-select RUF100 test_B010.py
test_B010.py:7:75: RUF100 Unused `noqa` directive for: B010
Found 1 error(s).
1 potentially fixable with the --fix option.
```

```python
$ cat test_B010.py 
def test():
    def local_func(self):
        return self

    setattr(local_func, "__comment_section__", "Local function comment")  # noqa: B010
    return local_func
```


---

_Label `bug` added by @charliermarsh on 2022-12-24 18:18_

---

_Comment by @charliermarsh on 2022-12-24 18:18_

Thanks! Will fix.

---

_Referenced in [PyCQA/flake8-bugbear#326](../../PyCQA/flake8-bugbear/issues/326.md) on 2022-12-24 18:19_

---

_Comment by @charliermarsh on 2022-12-24 18:46_

This actually looks right to me -- are you enabling the `B` checks? They're not enabled by default (only `E` and `F`).

<img width="868" alt="Screen Shot 2022-12-24 at 1 45 41 PM" src="https://user-images.githubusercontent.com/1309177/209447987-57cd0db7-3071-4d60-b883-0f6683eee90e.png">


---

_Comment by @ghuls on 2022-12-24 19:03_

> This actually looks right to me -- are you enabling the `B` checks? They're not enabled by default (only `E` and `F`).

Ah, I didn't know this. The warning message gave me the impression that it was enabled.



---

_Comment by @charliermarsh on 2022-12-24 19:04_

Yeah, I can improve the message there.

---

_Referenced in [astral-sh/ruff#1364](../../astral-sh/ruff/issues/1364.md) on 2022-12-24 19:09_

---

_Comment by @charliermarsh on 2022-12-24 19:09_

Tracking here: https://github.com/charliermarsh/ruff/issues/1364.

---

_Closed by @charliermarsh on 2022-12-24 19:09_

---
