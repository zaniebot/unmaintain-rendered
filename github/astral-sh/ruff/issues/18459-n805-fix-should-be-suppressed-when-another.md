---
number: 18459
title: N805 fix should be suppressed when another variable is using the recommended name
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-06-04T13:26:40Z
updated_at: 2025-06-11T05:58:56Z
url: https://github.com/astral-sh/ruff/issues/18459
synced_at: 2026-01-07T13:12:16-06:00
---

# N805 fix should be suppressed when another variable is using the recommended name

---

_Issue opened by @dscorbett on 2025-06-04 13:26_

### Summary

The fix for [`invalid-first-argument-name-for-method` (N805)](https://docs.astral.sh/ruff/rules/invalid-first-argument-name-for-method/) should be suppressed when the recommended parameter name is already taken by another variable used within that scope.

```console
$ cat >n805.py <<'# EOF'
class C:
    def f(this):
        self = type(this).__name__
        print(self, this.__sizeof__())
C().f()
# EOF

$ python n805.py
C 16

$ ruff --isolated check n805.py  --select N805 --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ cat n805.py
class C:
    def f(self):
        self = type(self).__name__
        print(self, self.__sizeof__())
C().f()

$ python n805.py
C 42
```

### Version

ruff 0.11.12 (aee3af0f7 2025-05-29)

---

_Label `bug` added by @ntBre on 2025-06-04 13:29_

---

_Label `fixes` added by @ntBre on 2025-06-04 13:29_

---

_Label `help wanted` added by @ntBre on 2025-06-04 13:29_

---

_Referenced in [astral-sh/ruff#18472](../../astral-sh/ruff/pulls/18472.md) on 2025-06-04 20:40_

---

_Closed by @MichaReiser on 2025-06-11 05:58_

---
