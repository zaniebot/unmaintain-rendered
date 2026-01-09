---
number: 20171
title: A003 misses some attributes referenced within the class scope
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
assignees: []
created_at: 2025-08-31T01:15:36Z
updated_at: 2025-09-22T22:12:46Z
url: https://github.com/astral-sh/ruff/issues/20171
synced_at: 2026-01-07T13:12:16-06:00
---

# A003 misses some attributes referenced within the class scope

---

_Issue opened by @dscorbett on 2025-08-31 01:15_

### Summary

[`builtin-attribute-shadowing` (A003)](https://docs.astral.sh/ruff/rules/builtin-attribute-shadowing/) is only emitted for attributes that “are referenced from within the class scope”, but that seems to only include references in annotations. There are false negatives for attributes used within the class scope in decorators, in argument defaults, and in other attributes’ definitions, as in `property`, `id`, and `bin` respectively in [the example below](https://play.ruff.rs/d9ec5da2-a26d-4ac1-a40d-e20d3c8f446e).
```console
$ cat >a003.py <<'# EOF'
class C:
    @staticmethod
    def property(f): return f
    id = 1
    @[property][0]
    def f(self, x=[id]): return x
    bin = 2
    foo = [bin]
print(C().f(), C().foo)
# EOF

$ python a003.py
[1] [2]

$ ruff --isolated check a003.py --select A003
All checks passed!

$ flake8 --select A a003.py
a003.py:3:5: A003 class attribute "property" is shadowing a Python builtin
a003.py:4:5: A003 class attribute "id" is shadowing a Python builtin
a003.py:7:5: A003 class attribute "bin" is shadowing a Python builtin

```

### Version

ruff 0.12.11 (c2bc15bc1 2025-08-28)

---

_Referenced in [astral-sh/ruff#20178](../../astral-sh/ruff/pulls/20178.md) on 2025-08-31 15:33_

---

_Label `bug` added by @ntBre on 2025-09-01 15:05_

---

_Label `rule` added by @ntBre on 2025-09-01 15:05_

---

_Closed by @ntBre on 2025-09-22 22:12_

---
