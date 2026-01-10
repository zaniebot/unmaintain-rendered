```yaml
number: 14704
title: UP009 fix changes a file from UTF-8 to a different declared encoding
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
  - help wanted
assignees: []
created_at: 2024-12-01T22:53:00Z
updated_at: 2024-12-11T10:30:42Z
url: https://github.com/astral-sh/ruff/issues/14704
synced_at: 2026-01-10T11:09:56Z
```

# UP009 fix changes a file from UTF-8 to a different declared encoding

---

_Issue opened by @dscorbett on 2024-12-01 22:53_

The fix for [`utf8-encoding-declaration` (UP009)](https://docs.astral.sh/ruff/rules/utf8-encoding-declaration/) in Ruff 0.8.1 changes the file’s declared encoding when the redundant UTF-8 encoding declaration is followed by a non-UTF-8 encoding declaration. In that case, the UTF-8 declaration is not completely redundant, because it blocks the following declaration from having an effect. The fix should insert up to 2 blank lines so the other declarations have no effect, or the fix should delete the other declarations, or the check should not report a violation.

Example of a syntax error:
```console
$ printf '\xef\xbb\xbf# coding: utf-8\n# coding: ascii\nprint("success")\n' >up009_1.py
$ python up009_1.py
success
$ ruff check --isolated --select UP009 up009_1.py --fix
Found 1 error (1 fixed, 0 remaining).
$ python up009_1.py
SyntaxError: encoding problem: ascii with BOM
```

Example of changed behavior:
```console
$ printf '# coding: utf-8\n# coding: latin-1\nprint("\xc3\xa5")\n' >up009_2.py
$ python up009_2.py
å
$ ruff check --isolated --select UP009 up009_2.py --fix
Found 1 error (1 fixed, 0 remaining).
$ python up009_2.py
Ã¥
```

---

_Label `bug` added by @AlexWaygood on 2024-12-01 23:07_

---

_Label `fixes` added by @AlexWaygood on 2024-12-01 23:07_

---

_Label `fixes` removed by @AlexWaygood on 2024-12-01 23:08_

---

_Label `rule` added by @AlexWaygood on 2024-12-01 23:08_

---

_Comment by @AlexWaygood on 2024-12-01 23:11_

> or the check should not report a violation.

I'd vote for this option. The rule's purpose is to flag unnecessary UTF-8 encoding declarations. Clearly in some cases it's erroneously flagging encodings which are, in fact, necessary :-)

---

_Label `help wanted` added by @AlexWaygood on 2024-12-01 23:11_

---

_Comment by @MichaReiser on 2024-12-02 07:55_

Somewhat related to https://github.com/astral-sh/ruff/issues/6791

---

_Closed by @MichaReiser on 2024-12-11 10:30_

---
