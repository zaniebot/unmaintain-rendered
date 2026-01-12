```yaml
number: 15776
title: FURB168 false negative and false positives
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
assignees: []
created_at: 2025-01-27T20:58:56Z
updated_at: 2025-01-31T11:34:58Z
url: https://github.com/astral-sh/ruff/issues/15776
synced_at: 2026-01-12T15:54:54Z
```

# FURB168 false negative and false positives

---

_@dscorbett_

### Description

[`isinstance-type-none` (FURB168)](https://docs.astral.sh/ruff/rules/isinstance-type-none/) has a false negative and two false positives in Ruff 0.9.3.

The rule only triggers when the first argument to `isinstance` is an identifier. This is a false negative because any expression would work in that context.
```console
$ cat >furb168_1.py <<'#EOF'
d = {"x": 1}
print(isinstance(d.get("x"), type(None)))
#EOF

$ python furb168_1.py
False

$ ruff --isolated check --preview --select FURB168 furb168_1.py
All checks passed!
```
If the expression consists purely of certain literals and operators, like `1 + 1`, applying the FURB168 fix would produce a warning like `SyntaxWarning: "is" with 'int' literal. Did you mean "=="?`. That might be a reason the rule is currently so restricted, but I think it is better to apply the fix: the code is suspect either way but at least with the fix the user gets an explicit warning.

The first false positive is that the rule does not check whether `type` is the built-in `type`. (It does correctly check `isinstance`.)
```console
$ cat >furb168_2.py <<'#EOF'
def type(x):
    return int
i = 1
print(isinstance(i, type(None)))
#EOF

$ python furb168_2.py
True

$ ruff --isolated check --preview --select FURB168 furb168_2.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat furb168_2.py
def type(x):
    return int
i = 1
print(i is None)

$ python furb168_2.py
False
```

The second false positive is that `None | None` is accepted as meaning `type(None)`. That raises a `TypeError` which the fix suppresses.
```
$ cat >furb168_3.py <<'#EOF'
print(isinstance(abs, None | None))
#EOF

$ python furb168_3.py
Traceback (most recent call last):
  File "furb168_3.py", line 1, in <module>
    print(isinstance(abs, None | None))
                          ~~~~~^~~~~~
TypeError: unsupported operand type(s) for |: 'NoneType' and 'NoneType'

$ ruff --isolated check --preview --select FURB168 furb168_3.py --fix
Found 1 error (1 fixed, 0 remaining).

$ python furb168_3.py
False
```

---

_Comment by @dylwil3 on 2025-01-27 21:46_

Thank you! I'm more interested in resolving the false positives than the false negative.

---

_Label `bug` added by @dylwil3 on 2025-01-27 21:47_

---

_Label `rule` added by @dylwil3 on 2025-01-27 21:47_

---

_Closed by @AlexWaygood on 2025-01-31 11:34_

---
