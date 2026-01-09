---
number: 20562
title: "F821 false positive for `__class__` in lambda expression in class definition"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-09-24T21:39:43Z
updated_at: 2025-10-24T14:07:21Z
url: https://github.com/astral-sh/ruff/issues/20562
synced_at: 2026-01-07T13:12:16-06:00
---

# F821 false positive for `__class__` in lambda expression in class definition

---

_Issue opened by @dscorbett on 2025-09-24 21:39_

### Summary

[`undefined-name` (F821)](https://docs.astral.sh/ruff/rules/undefined-name/) has a false positive for `__class__` in a lambda expression within a class definition. [Example](https://play.ruff.rs/abe91404-af96-4ccf-aebb-bd92daa872d4):
```console
$ cat >f821.py <<'# EOF'
class C:
    f = lambda self: __class__
print(C().f().__name__)
# EOF

$ python f821.py
C

$ ruff --isolated check f821.py --select F821 --output-format concise -q
f821.py:2:22: F821 Undefined name `__class__`
```

### Version

ruff 0.13.1 (706be0a6e 2025-09-18)

---

_Comment by @ntBre on 2025-09-24 22:41_

Ah, thank you! I'm guessing we need to do something like we did for functions in #20024 
https://github.com/astral-sh/ruff/blob/5996dc3f4fd827cb26a17ac5c27481b0ce3277fe/crates/ruff_linter/src/checkers/ast/mod.rs#L1124-L1140

---

_Label `bug` added by @ntBre on 2025-09-24 22:41_

---

_Label `help wanted` added by @ntBre on 2025-09-24 22:41_

---

_Referenced in [astral-sh/ruff#20564](../../astral-sh/ruff/pulls/20564.md) on 2025-09-25 03:38_

---

_Closed by @ntBre on 2025-10-24 14:07_

---
