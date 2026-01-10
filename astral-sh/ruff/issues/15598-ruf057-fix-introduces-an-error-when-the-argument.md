```yaml
number: 15598
title: RUF057 fix introduces an error when the argument contains a newline
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - preview
assignees: []
created_at: 2025-01-20T02:33:48Z
updated_at: 2025-01-24T10:34:58Z
url: https://github.com/astral-sh/ruff/issues/15598
synced_at: 2026-01-10T11:09:57Z
```

# RUF057 fix introduces an error when the argument contains a newline

---

_Issue opened by @dscorbett on 2025-01-20 02:33_

The fix for [`unnecessary-round` (RUF057)](https://docs.astral.sh/ruff/rules/unnecessary-round/)  in Ruff 0.9.2 can introduce a syntax error or change behavior when the argument to `round` contains a newline.

Here is a crash:
```console
$ cat ruf057_1.py
round(-
1)

$ ruff --isolated check --fix --preview --select RUF057 ruf057_1.py

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `ruf057_1.py`, the rule codes RUF057, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

ruf057_1.py:1:1: RUF057 Value being rounded is already an integer
  |
1 | / round(-
2 | | 1)
  | |__^ RUF057
  |
  = help: Remove unnecessary `round` call

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

Here is a syntax error:
```console
$ cat ruf057_2.py
round(1
* 1)

$ ruff --isolated check --fix --preview --select RUF057 ruf057_2.py
Found 1 error (1 fixed, 0 remaining).

$ cat ruf057_2.py
1
* 1

$ python ruf057_2.py
  File "ruf057_2.py", line 2
    * 1
    ^^^
SyntaxError: can't use starred expression here
```

Here is a runtime behavior change:
```console
$ cat ruf057_3.py
x = round(1
+ 1)
print(x)

$ python ruf057_3.py
2

$ ruff --isolated check --fix --preview --select RUF057 ruf057_3.py
Found 1 error (1 fixed, 0 remaining).

$ cat ruf057_3.py
x = 1
+ 1
print(x)

$ python ruf057_3.py
1
```

---

_Comment by @dhruvmanila on 2025-01-20 05:01_

Thanks for the examples!

We need to preserve the parentheses in these cases. The code in which ruff crashes also has the same change as your other examples:

```
error: Fix introduced a syntax error in `-` with rule codes RUF057: Expected an expression at byte range 1..2
---
-
1
---
```

cc @InSyncWithFoo in case you're interested

---

_Label `bug` added by @dhruvmanila on 2025-01-20 05:01_

---

_Label `fixes` added by @dhruvmanila on 2025-01-20 05:01_

---

_Label `preview` added by @dylwil3 on 2025-01-24 00:09_

---

_Closed by @dylwil3 on 2025-01-24 10:34_

---
