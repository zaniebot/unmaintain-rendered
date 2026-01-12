```yaml
number: 17606
title: UP018 fix should add spaces between tokens
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-04-24T13:13:10Z
updated_at: 2025-05-07T07:34:09Z
url: https://github.com/astral-sh/ruff/issues/17606
synced_at: 2026-01-12T15:54:56Z
```

# UP018 fix should add spaces between tokens

---

_@dscorbett_

### Summary

The fix for [`native-literals` (UP018)](https://docs.astral.sh/ruff/rules/native-literals/) should add spaces between tokens as necessary to avoid syntax errors and other problems.

Reverted syntax error:
```console
$ cat >up018_1.py <<'# EOF'
bool(True)and None
# EOF

$ ruff --isolated check up018_1.py --select UP018 --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

Runtime `SyntaxWarning`s:
```console
$ cat >up018_2.py <<'# EOF'
int(1)and None
float(1.)and None
# EOF

$ ruff --isolated check up018_2.py --select UP018 --fix
Found 2 errors (2 fixed, 0 remaining).

$ cat up018_2.py
1and None
1.and None

$ python up018_2.py
up018_2.py:1: SyntaxWarning: invalid decimal literal
  1and None
up018_2.py:2: SyntaxWarning: invalid decimal literal
  1.and None
```

Runtime behavior change:
```console
$ cat >up018_3.py <<'# EOF'
bool(True)and()
# EOF

$ ruff --isolated check up018_3.py --select UP018 --fix
Found 1 error (1 fixed, 0 remaining).

$ cat up018_3.py
Trueand()

$ python up018_3.py 
Traceback (most recent call last):
  File "up018_3.py", line 1, in <module>
    Trueand()
    ^^^^^^^
NameError: name 'Trueand' is not defined
```


### Version

ruff 0.11.6 (fcd50a049 2025-04-17)

---

_Label `bug` added by @AlexWaygood on 2025-04-24 13:14_

---

_Label `fixes` added by @AlexWaygood on 2025-04-24 13:14_

---

_Label `help wanted` added by @AlexWaygood on 2025-04-24 13:14_

---

_Closed by @MichaReiser on 2025-05-07 07:34_

---
