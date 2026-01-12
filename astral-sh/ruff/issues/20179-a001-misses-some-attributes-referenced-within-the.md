```yaml
number: 20179
title: A001 misses some attributes referenced within the class scope
type: issue
state: open
author: dscorbett
labels:
  - bug
  - rule
assignees: []
created_at: 2025-08-31T18:47:31Z
updated_at: 2025-11-08T20:31:10Z
url: https://github.com/astral-sh/ruff/issues/20179
synced_at: 2026-01-12T15:54:57Z
```

# A001 misses some attributes referenced within the class scope

---

_@dscorbett_

### Summary

[`builtin-variable-shadowing` (A001)](https://docs.astral.sh/ruff/rules/builtin-variable-shadowing/) ignores some bindings in class scope which it checks in other scopes. [Example](https://play.ruff.rs/1ab5858c-b390-4f6a-9f89-0b98ef1cf5d9):
```console
$ cat >a001.py <<'# EOF'
class C:
    for id in [1]: pass
    def f1(self, x=id): return x

    (id := 2)
    def f2(self, x=id): return x

    from contextlib import nullcontext
    with nullcontext() as id: pass
    def f3(self, x=id): return x

c = C()
print(c.f1(), c.f2(), c.f3())
# EOF

$ python a001.py
1 2 None

$ ruff --isolated check a001.py --select A001
All checks passed!

$ flake8 --select A a001.py
a001.py:2:5: A001 variable "id" is shadowing a Python builtin
a001.py:5:6: A001 variable "id" is shadowing a Python builtin
a001.py:9:5: A001 variable "id" is shadowing a Python builtin
```

It might make more sense to report these for [`builtin-attribute-shadowing` (A003)](https://docs.astral.sh/ruff/rules/builtin-attribute-shadowing/), but A001 is how the upstream plugin categorizes them. Either way, they are false negatives.

### Version

ruff 0.12.11 (c2bc15bc1 2025-08-28)

---

_Label `bug` added by @ntBre on 2025-09-01 14:59_

---

_Label `rule` added by @ntBre on 2025-09-01 14:59_

---

_Comment by @rrhodes on 2025-11-08 20:15_

Hi @dscorbett, which version of Flake8 were you using for the example provided? I tried repeating your steps with `flake8` on `7.3.0` and didn't receive any of the output shared (i.e. the `variable "id" is shadowing a Python builtin` statements).

---

_Comment by @dscorbett on 2025-11-08 20:31_

This is with Flake8 version 7.3.0 and [flake8-builtins](https://github.com/gforcada/flake8-builtins) version 3.1.0.

---
