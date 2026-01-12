```yaml
number: 18675
title: PLW0108 fix mishandles assignment expressions
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-06-14T17:20:06Z
updated_at: 2025-06-26T20:56:18Z
url: https://github.com/astral-sh/ruff/issues/18675
synced_at: 2026-01-12T15:54:56Z
```

# PLW0108 fix mishandles assignment expressions

---

_@dscorbett_

### Summary

The fix for [`unnecessary-lambda` (PLW0108)](https://docs.astral.sh/ruff/rules/unnecessary-lambda/) introduces a syntax error when the callable in the top level of the lambda expression is an assignment expression. It should be parenthesized.

```console
$ cat >plw0108_1.py <<'# EOF'
f = lambda x: (string := str)(x)
print(f(0))
# EOF

$ python plw0108_1.py
0

$ ruff --isolated check plw0108_1.py --select PLW0108 --preview --unsafe-fixes --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

The fix can also change behavior when a variable bound by an assignment expression is one of the lambda expression’s parameters. The fix is already documented as unsafe because “the lambda body itself could contain an effect”, but in this case, the body is guaranteed to contain an effect, and there is no straightforward way to refactor it, so I recommend the fix be suppressed.

```console
$ cat >plw0108_2.py <<'# EOF'
x = None
f = lambda x: ((x := 1) and str)(x)
print(f(0))
print(x)
# EOF

$ python plw0108_2.py
1
None

$ ruff --isolated check plw0108_2.py --select PLW0108 --preview --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ cat plw0108_2.py
x = None
f = (x := 1) and str
print(f(0))
print(x)

$ python plw0108_2.py
0
1
```

### Version

ruff 0.11.13 (5faf72a4d 2025-06-05)

---

_Label `bug` added by @ntBre on 2025-06-14 18:51_

---

_Label `fixes` added by @ntBre on 2025-06-14 18:51_

---

_Label `help wanted` added by @ntBre on 2025-06-14 18:51_

---

_Closed by @ntBre on 2025-06-26 20:56_

---
