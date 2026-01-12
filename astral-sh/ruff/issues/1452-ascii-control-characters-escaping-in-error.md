```yaml
number: 1452
title: ASCII control characters escaping in error messages
type: issue
state: closed
author: AA-Turner
labels:
  - bug
assignees: []
created_at: 2022-12-29T23:02:39Z
updated_at: 2022-12-31T03:11:02Z
url: https://github.com/astral-sh/ruff/issues/1452
synced_at: 2026-01-12T15:54:41Z
```

# ASCII control characters escaping in error messages

---

_@AA-Turner_

Line feed literals, carriage returns, and tabs are all printed as their actual characters rather than the escaped sequence in error messages.

Note in the reproducer that a new line seperates '1' and '2', a sequence of spaces seperates '3' and '4', and that '6' has been printed at the start of the line as the terminal's cursor has interpreted the carriage return to overprint text.

Reproducer:

```ps1con
(sphinx) PS S:\Development\sphinx> ruff -V
ruff 0.0.199
(sphinx) PS S:\Development\sphinx> type bug.py
if token == "1\n2":
    pass

if token == "3\t4":
    pass

if token == "5\r6":
    pass
(sphinx) PS S:\Development\sphinx> ruff bug.py --select S105
bug.py:1:13: S105 Possible hardcoded password: `"1
2"`
bug.py:4:13: S105 Possible hardcoded password: `"3      4"`
6"`.py:7:13: S105 Possible hardcoded password: `"5
Found 3 error(s).
```

A

---

_Label `bug` added by @charliermarsh on 2022-12-29 23:05_

---

_Comment by @not-my-profile on 2022-12-30 07:10_

Sidenote: We do not want to escape all error message arguments: e.g. the messages (`.msg`) of the new `banned-api` setting should be interpolated into the error message without being escaped (so that they can contain newlines).

---

_Closed by @charliermarsh on 2022-12-31 03:11_

---
