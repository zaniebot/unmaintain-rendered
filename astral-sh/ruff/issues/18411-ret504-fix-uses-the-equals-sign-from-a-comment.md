```yaml
number: 18411
title: RET504 fix uses the equals sign from a comment
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-06-01T15:03:50Z
updated_at: 2025-06-11T13:38:44Z
url: https://github.com/astral-sh/ruff/issues/18411
synced_at: 2026-01-12T15:54:56Z
```

# RET504 fix uses the equals sign from a comment

---

_@dscorbett_

### Summary

The fix for [`unnecessary-assign` (RET504)](https://docs.astral.sh/ruff/rules/unnecessary-assign/) assumes that the first equals sign in the assignment statement is the one that does the assignment. However, if the first equals sign is in a comment, the fix can fail because of a syntax error. The fix should seek the first equals sign token, not the first equals sign character.
```console
$ cat >ret504.py <<'# EOF'
def f():
    (#=
    x) = 1
    return x
# EOF

$ ruff --isolated check ret504.py --select RET504 --unsafe-fixes --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

### Version

ruff 0.11.12 (aee3af0f7 2025-05-29)

---

_Label `bug` added by @MichaReiser on 2025-06-02 07:11_

---

_Label `fixes` added by @MichaReiser on 2025-06-02 07:11_

---

_Label `help wanted` added by @MichaReiser on 2025-06-02 07:11_

---

_Closed by @MichaReiser on 2025-06-11 13:38_

---
