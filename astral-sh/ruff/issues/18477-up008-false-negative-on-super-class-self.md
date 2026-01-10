```yaml
number: 18477
title: "UP008 false negative on `super(__class__, self)`"
type: issue
state: closed
author: dscorbett
labels:
  - rule
  - help wanted
assignees: []
created_at: 2025-06-05T12:35:10Z
updated_at: 2025-06-12T06:52:46Z
url: https://github.com/astral-sh/ruff/issues/18477
synced_at: 2026-01-10T11:09:58Z
```

# UP008 false negative on `super(__class__, self)`

---

_Issue opened by @dscorbett on 2025-06-05 12:35_

### Summary

The documentation for [`super-call-with-parameters` (UP008)](https://docs.astral.sh/ruff/rules/super-call-with-parameters/) says:
> In Python 3, `super` can be invoked without any arguments when: (1) the first argument is `__class__`, and (2) the second argument is equivalent to the first argument of the enclosing method.

However, the rule only applies when the first argument is the class by name. UP008 should check for `__class__` too.
```console
$ cat >up008.py <<'# EOF'
class A:
    def foo(self):
        pass
class B(A):
    def bar(self):
        super(__class__, self).foo()
# EOF

$ ruff --isolated check up008.py --select UP008
All checks passed!
```

### Version

ruff 0.11.12 (aee3af0f7 2025-05-29)

---

_Label `rule` added by @ntBre on 2025-06-05 12:41_

---

_Label `help wanted` added by @ntBre on 2025-06-05 12:41_

---

_Closed by @MichaReiser on 2025-06-12 06:52_

---
