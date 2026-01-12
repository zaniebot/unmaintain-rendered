```yaml
number: 21082
title: Raw strings break the FLY002 fix
type: issue
state: open
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-10-27T00:02:48Z
updated_at: 2025-10-27T13:54:26Z
url: https://github.com/astral-sh/ruff/issues/21082
synced_at: 2026-01-12T15:54:57Z
```

# Raw strings break the FLY002 fix

---

_@dscorbett_

### Summary

When the argument to `str.join` contains only string literals and the first is raw, the fix for [`static-join-to-f-string` (FLY002)](https://docs.astral.sh/ruff/rules/static-join-to-f-string/) can introduce a syntax error or change the program’s behavior.

If a subsequent string isn’t raw, its contents will be inserted into the fix’s string, regardless of syntax errors. [Example](https://play.ruff.rs/372f5506-c7bf-41ba-8ef7-fc3eafe725a8):
```console
$ cat >fly002_1.py <<'# EOF'
"".join((r"", '"'))
# EOF

$ ruff check --isolated fly002_1.py --select FLY002 --unsafe-fixes --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

[Another example](https://play.ruff.rs/503176c6-9a2e-428c-befc-1130af96d1e0):
```console
$ cat >fly002_2.py <<'# EOF'
"".join((r"", "\\"))
# EOF

$ ruff check --isolated fly002_2.py --select FLY002 --unsafe-fixes --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

[Another example](https://play.ruff.rs/83a55ba4-cf6f-4bd1-85ed-6824692f6dbd):
```console
$ cat >fly002_3.py <<'# EOF'
"".join((r"", "\0"))
# EOF

$ ruff check --isolated fly002_3.py --select FLY002 --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ xxd fly002_3.py
00000000: 7222 0022 0a                             r".".

$ python fly002_3.py 2>&1 | tail -n 1
SyntaxError: source code cannot contain null bytes
```

It can also change the program’s behavior. [Example](https://play.ruff.rs/35984440-7add-4737-a205-77af0417c5d2):
```console
$ cat >fly002_4.py <<'# EOF'
print("".join((r"", r'".__class__.__name__, 0xA, sep="~')))
# EOF

$ python fly002_4.py
".__class__.__name__, 0xA, sep="~

$ ruff check --isolated fly002_4.py --select FLY002 --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ cat fly002_4.py
print(r"".__class__.__name__, 0xA, sep="~")

$ python fly002_4.py
str~10
```

If any string contains a carriage return via an escape sequence, the fix emits a multiline string containing a literal carriage return. Literal carriage returns are interpreted as line feeds, so this changes the program’s behavior. [Example](https://play.ruff.rs/a0fee72c-bb57-4a46-8410-672fd9039c5d):
```console
$ cat >fly002_5.py <<'# EOF'
print(repr("".join((r"", "\r"))))
# EOF

$ python fly002_5.py
'\r'

$ ruff check --isolated fly002_5.py --select FLY002 --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ xxd fly002_5.py
00000000: 7072 696e 7428 7265 7072 2872 2222 220d  print(repr(r""".
00000010: 2222 2229 290a                           """)).

$ python fly002_5.py
'\n'
```

### Version

ruff 0.14.2 (83a3bc4ee 2025-10-23)

---

_Label `bug` added by @ntBre on 2025-10-27 13:54_

---

_Label `fixes` added by @ntBre on 2025-10-27 13:54_

---
