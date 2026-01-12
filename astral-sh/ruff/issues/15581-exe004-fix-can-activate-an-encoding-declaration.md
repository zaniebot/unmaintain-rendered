```yaml
number: 15581
title: EXE004 fix can activate an encoding declaration
type: issue
state: open
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-01-19T00:35:03Z
updated_at: 2025-01-20T06:23:32Z
url: https://github.com/astral-sh/ruff/issues/15581
synced_at: 2026-01-12T15:54:54Z
```

# EXE004 fix can activate an encoding declaration

---

_@dscorbett_

The fix for [`shebang-leading-whitespace` (EXE004)](https://docs.astral.sh/ruff/rules/shebang-leading-whitespace/) in Ruff 0.9.2 removes the white space before the shebang. If that white space contains a newline and the line after the shebang contains an encoding declaration, the fix moves the encoding declaration up to the second line, where it has an effect and can change the program’s behavior.
```console
$ printf '\n#!/usr/bin/env python\n#coding=latin-1\nprint("\xc3\xa5")\n' >exe004_1.py

$ python exe004_1.py
å

$ ruff --isolated check --fix --select EXE004 exe004_1.py
Found 1 error (1 fixed, 0 remaining).

$ python exe004_1.py
Ã¥
```
The intended behavior is unclear for a script like this (if the original behavior was intended, the fix should move the shebang before the blank lines instead of deleting the blank lines; otherwise, the current fix is correct) but at least the fix should not be marked safe.

A similar behavior change occurs when the leading white space contains two newlines and the shebang line contains an encoding declaration.
```console
$ printf '\n\n#!/usr/bin/env -Spython3.12\\coding=latin-1\nprint("\xc3\xa5")\n' >exe004_2.py

$ python exe004_2.py
å

$ ruff --isolated check --fix --select EXE004 exe004_2.py
Found 1 error (1 fixed, 0 remaining).

$ python exe004_2.py
Ã¥
```

---

_Label `bug` added by @MichaReiser on 2025-01-19 09:57_

---

_Comment by @MichaReiser on 2025-01-19 09:58_

Not marking it as safe seems reasonable 

---

_Label `fixes` added by @dhruvmanila on 2025-01-20 06:23_

---
