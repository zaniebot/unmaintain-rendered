---
number: 3273
title: Errors set as unfixable are reported as fixable
type: issue
state: closed
author: MarkTuddenham
labels:
  - bug
assignees: []
created_at: 2023-02-28T11:52:17Z
updated_at: 2023-05-18T16:00:32Z
url: https://github.com/astral-sh/ruff/issues/3273
synced_at: 2026-01-07T13:12:14-06:00
---

# Errors set as unfixable are reported as fixable

---

_Issue opened by @MarkTuddenham on 2023-02-28 11:52_


Expect `ruff` to tell me which errors it will fix when called with `--fix`.

`test.py`:
```python
# import os
```

```bash
> ruff check test.py --isolated --select ERA001 --unfixable ERA001
test.py:1:1: ERA001 [*] Found commented-out code
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```
but this error is known to be "unfixable" and won't be fixed by `ruff check test.py --isolated --select ERA001 --unfixable ERA001 --fix` which is not obvious if `unfixable` is set in a config file.

A better output might be
```bash
> ruff check test.py --isolated --select ERA001 --unfixable ERA001
test.py:1:1: ERA001 Found commented-out code
Found 1 error.
```
or
```bash
> ruff check test.py --isolated --select ERA001 --unfixable ERA001
test.py:1:1: ERA001 [x] Found commented-out code
Found 1 error.
[x] 1 potentially fixable defined as unfixable.
```
---
```bash
> ruff --version
ruff 0.0.253
```


---

_Label `bug` added by @charliermarsh on 2023-02-28 15:40_

---

_Comment by @charliermarsh on 2023-02-28 15:40_

Yeah good call. That diagnostic output doesn't take the user's fixability settings into account.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-21 23:15_

---

_Referenced in [astral-sh/ruff#4239](../../astral-sh/ruff/pulls/4239.md) on 2023-05-09 15:44_

---

_Comment by @charliermarsh on 2023-05-18 16:00_

This was fixed by #4239.

---

_Closed by @charliermarsh on 2023-05-18 16:00_

---
