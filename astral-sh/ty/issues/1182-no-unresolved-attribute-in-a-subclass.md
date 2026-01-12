```yaml
number: 1182
title: "no [unresolved-attribute] in a subclass"
type: issue
state: closed
author: andythomas
labels: []
assignees: []
created_at: 2025-09-14T17:04:11Z
updated_at: 2025-09-15T07:38:07Z
url: https://github.com/astral-sh/ty/issues/1182
synced_at: 2026-01-12T15:54:24Z
```

# no [unresolved-attribute] in a subclass

---

_@andythomas_

### Summary

Please consider the following stripped down example:

```python
class A:
    def __init__(self):
        pass

    def a(self):
        pass


class B(A):
    def __init__(self):
        self._a()
        self.a()
        super().__init__()


var = B()
var._a()
var.a()
```

From `ty`, I get only one error:

```
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                                            error[unresolved-attribute]: Type `B` has no attribute `_a`
  --> class_ty.py:18:1
   |
17 | var = B()
18 | var._a()
   | ^^^^^^
19 | var.a()
   |
info: rule `unresolved-attribute` is enabled by default

Found 1 diagnostic
```

when I run `pyrefly` as a sanity check, I get 

```
ERROR Object of class `B` has no attribute `_a` [missing-attribute]
  --> /Users/thomasa/Seafile/Work/Projects/matr1x/class_ty.py:11:9
   |
11 |         self._a()
   |         ^^^^^^^
   |
ERROR Object of class `B` has no attribute `_a` [missing-attribute]
  --> /Users/thomasa/Seafile/Work/Projects/matr1x/class_ty.py:17:1
   |
17 | var._a()
   | ^^^^^^
   |
 INFO 2 errors
```

so I imply my example is okay. For what it is worth, `mypy` also only finds the second error, while `pyright`also finds both.

### Version

ty 0.0.1-alpha.20 (f41f00af1 2025-09-03)

---

_Comment by @sharkdp on 2025-09-15 07:25_

Thank you for reporting this.

ty does not emit a diagnostic in line 11, because we currently still infer `Unknown` for an unannotated `self` in methods. We hope to fix this soon. See #159 

---

_Closed by @sharkdp on 2025-09-15 07:25_

---

_Comment by @andythomas on 2025-09-15 07:38_

You are always so quick to respond. Thank you so much for the explanation!

---
