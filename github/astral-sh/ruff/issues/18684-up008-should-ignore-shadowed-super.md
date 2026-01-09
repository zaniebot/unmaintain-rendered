---
number: 18684
title: "UP008 should ignore shadowed `super`"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-06-15T12:27:31Z
updated_at: 2025-06-16T19:09:32Z
url: https://github.com/astral-sh/ruff/issues/18684
synced_at: 2026-01-07T13:12:16-06:00
---

# UP008 should ignore shadowed `super`

---

_Issue opened by @dscorbett on 2025-06-15 12:27_

### Summary

[`super-call-with-parameters` (UP008)](https://docs.astral.sh/ruff/rules/super-call-with-parameters/) should not apply when `super` isn’t the built-in `super`.

```console
$ cat >up008.py <<'# EOF'
class C:
    def f(self):
        super = print
        super(C, self)
C().f()
# EOF

$ python up008.py
<class '__main__.C'> <__main__.C object at 0x104a1abd0>

$ ruff --isolated check up008.py --select UP008 --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ python up008.py


```

### Version

ruff 0.11.13 (5faf72a4d 2025-06-05)

---

_Label `bug` added by @ntBre on 2025-06-15 16:53_

---

_Label `fixes` added by @ntBre on 2025-06-15 16:53_

---

_Label `help wanted` added by @ntBre on 2025-06-15 16:53_

---

_Referenced in [astral-sh/ruff#18688](../../astral-sh/ruff/pulls/18688.md) on 2025-06-15 21:01_

---

_Closed by @ntBre on 2025-06-16 19:09_

---
