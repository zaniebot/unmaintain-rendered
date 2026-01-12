```yaml
number: 17124
title: UP018 fix introduces a syntax error when the argument contains a newline
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-04-01T15:37:56Z
updated_at: 2025-04-24T06:48:03Z
url: https://github.com/astral-sh/ruff/issues/17124
synced_at: 2026-01-12T15:54:55Z
```

# UP018 fix introduces a syntax error when the argument contains a newline

---

_@dscorbett_

### Summary

The fix for [`native-literals` (UP018)](https://docs.astral.sh/ruff/rules/native-literals/) can introduce a syntax error when the argument contains a newline between a unary `+` or `-` and a number literal. It should wrap the expression with parentheses as needed to avoid the error.
```console
$ cat >up018.py <<'# EOF'
int(-
1)
# EOF

$ ruff --isolated check up018.py --select UP018 --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

### Version

ruff 0.11.2 (4773878ee 2025-03-21)

---

_Label `bug` added by @MichaReiser on 2025-04-01 16:14_

---

_Label `fixes` added by @AlexWaygood on 2025-04-01 17:53_

---

_Closed by @MichaReiser on 2025-04-24 06:48_

---
