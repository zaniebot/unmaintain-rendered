```yaml
number: 18353
title: "B010 should ignore the attribute `__debug__`"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-05-28T13:32:11Z
updated_at: 2025-05-28T20:24:53Z
url: https://github.com/astral-sh/ruff/issues/18353
synced_at: 2026-01-12T15:54:56Z
```

# B010 should ignore the attribute `__debug__`

---

_@dscorbett_

### Summary

[`set-attr-with-constant` (B010)](https://docs.astral.sh/ruff/rules/set-attr-with-constant/) should not apply when the attribute name is `"__debug__"`.

```console
$ cat >b010.py <<'# EOF'
x = type("C", (), {})
setattr(x, "__debug__", 0)
print(x.__debug__)
# EOF

$ python b010.py
0

$ ruff --isolated check b010.py --select B010 --fix
Found 1 error (1 fixed, 0 remaining).

$ python b010.py
  File "b010.py", line 2
    x.__debug__ = 0
    ^^^^^^^^^^^
SyntaxError: cannot assign to __debug__
```

### Version

ruff 0.11.11 (0397682f1 2025-05-22)

---

_Label `bug` added by @MichaReiser on 2025-05-28 13:34_

---

_Label `help wanted` added by @MichaReiser on 2025-05-28 13:34_

---

_Closed by @ntBre on 2025-05-28 20:24_

---
