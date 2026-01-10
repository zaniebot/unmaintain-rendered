```yaml
number: 15263
title: RUF046 fix is wrong when a newline appears before a call expression’s opening parenthesis
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
  - preview
assignees: []
created_at: 2025-01-05T03:08:04Z
updated_at: 2025-01-07T21:43:51Z
url: https://github.com/astral-sh/ruff/issues/15263
synced_at: 2026-01-10T11:09:56Z
```

# RUF046 fix is wrong when a newline appears before a call expression’s opening parenthesis

---

_Issue opened by @dscorbett on 2025-01-05 03:08_

#15230 categorized `Subscript` and `Call` as having their own brackets, but because their brackets do not surround the entire expressions, this was not a safe categorization for the purpose of [unnecessary-cast-to-int (RUF046)](https://docs.astral.sh/ruff/rules/unnecessary-cast-to-int/). The fix can change program behavior in Ruff 0.8.6.

```console
$ cat ruf046.py
x = int(round
(1))
print(x)

$ python ruf046.py
1

$ ruff check --isolated --preview --select RUF046 ruf046.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat ruf046.py
x = round
(1)
print(x)

$ python ruf046.py
<built-in function round>
```

---

_Label `bug` added by @MichaReiser on 2025-01-05 08:31_

---

_Label `help wanted` added by @MichaReiser on 2025-01-05 08:31_

---

_Label `fixes` added by @AlexWaygood on 2025-01-05 10:10_

---

_Label `preview` added by @AlexWaygood on 2025-01-05 18:13_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-01-05 21:00_

---

_Closed by @charliermarsh on 2025-01-07 21:43_

---

_Closed by @charliermarsh on 2025-01-07 21:43_

---
