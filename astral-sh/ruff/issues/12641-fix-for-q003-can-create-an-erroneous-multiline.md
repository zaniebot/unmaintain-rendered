```yaml
number: 12641
title: Fix for Q003 can create an erroneous multiline string
type: issue
state: open
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-08-02T18:52:44Z
updated_at: 2024-08-03T12:23:23Z
url: https://github.com/astral-sh/ruff/issues/12641
synced_at: 2026-01-10T11:09:54Z
```

# Fix for Q003 can create an erroneous multiline string

---

_Issue opened by @dscorbett on 2024-08-02 18:52_

The automated fix for [Q003](https://docs.astral.sh/ruff/rules/avoidable-escaped-quote/) can introduce a syntax error or change behavior.

It can introduce a syntax error when the affected string immediately follows an empty string with the opposite kind of quotation marks. The fix makes the three adjacent quotation marks be reinterpreted as the start of a multiline string. If the multiline string is not terminated, that is a syntax error.
```console
$ ruff --version
ruff 0.5.6
$ cat q003.py
print(''"\"")
$ ruff check --isolated --select Q003 q003.py --fix

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `q003.py`, the rule codes Q003, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

q003.py:1:9: Q003 Change outer quotes to avoid escaping inner quotes
  |
1 | print(''"\"")
  |         ^^^^ Q003
  |
  = help: Change outer quotes to avoid escaping inner quotes

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

If the multiline string introduced by the fix is terminated, the behavior of the program can change.
```console
$ cat q003.py
print(''"\""'''''')  # ''')
$ python q003.py
"
$ ruff check --isolated --select Q003 q003.py --fix
Found 1 error (1 fixed, 0 remaining).
$ python q003.py
"')  # 
```

The fix should insert a space before the modified string, if it is necessary based on the previous token.

---

_Label `docstring` added by @charliermarsh on 2024-08-02 21:43_

---

_Label `fixes` added by @charliermarsh on 2024-08-02 21:43_

---

_Label `docstring` removed by @charliermarsh on 2024-08-02 21:44_

---

_Label `bug` added by @AlexWaygood on 2024-08-03 12:23_

---
