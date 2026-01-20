```yaml
number: 22597
title: UP008 false negative in inner (nested) classes
type: issue
state: open
author: generalmimon
labels:
  - bug
  - rule
assignees: []
created_at: 2026-01-15T13:09:16Z
updated_at: 2026-01-20T22:43:49Z
url: https://github.com/astral-sh/ruff/issues/22597
synced_at: 2026-01-20T22:52:40Z
```

# UP008 false negative in inner (nested) classes

---

_@generalmimon_

### Summary

Playground link: https://play.ruff.rs/f9f8f76b-c468-4f4f-a3b8-0bca5850daf1

```python
class Base:
    def __init__(self, foo):
        self.foo = foo

class Outer(Base):
    def __init__(self, foo):
        super(Outer, self).__init__(foo)  # UP008 issued as expected

    class Inner(Base):
        def __init__(self, foo):
            super(Outer.Inner, self).__init__(foo)  # Bug: would also expect UP008, but it was not issued

o = Outer(5)
i = Outer.Inner(5)
```

---

By the way, Pylint's [`super-with-arguments`](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/super-with-arguments.html) also has a problem only with `super(Outer, self)`, but not with `super(Outer.Inner, self)`:

```console
$ pylint --version
pylint 4.0.4
astroid 4.0.3
Python 3.14.2 (tags/v3.14.2:df79316, Dec  5 2025, 17:18:21) [MSC v.1944 64 bit (AMD64)]
$ pylint --disable=all --enable=super-with-arguments test.py
************* Module test
test.py:7:8: R1725: Consider using Python 3 style super() without arguments (super-with-arguments)

-----------------------------------
Your code has been rated at 9.09/10
```

However, https://github.com/asottile/pyupgrade correctly upgrades both `super()` calls:

```console
$ pip freeze | grep -F pyupgrade
pyupgrade==3.21.2
$ cp test.py test-original.py
$ pyupgrade test.py
Rewriting test.py
$ git diff --no-index test-original.py test.py
diff --git 1/test-original.py 2/test.py
index 2f6d7cd2..65bf24ac 100644
--- 1/test-original.py
+++ 2/test.py
@@ -4,11 +4,11 @@ class Base:
 
 class Outer(Base):
     def __init__(self, foo):
-        super(Outer, self).__init__(foo)  # UP008 issued as expected
+        super().__init__(foo)  # UP008 issued as expected
 
     class Inner(Base):
         def __init__(self, foo):
-            super(Outer.Inner, self).__init__(foo)  # Bug: would also expect UP008, but it was not issued
+            super().__init__(foo)  # Bug: would also expect UP008, but it was not issued
 
 o = Outer(5)
 i = Outer.Inner(5)
```

### Version

ruff 0.14.11

---

_Comment by @ntBre on 2026-01-15 15:44_

Thanks for the report and the great write-up! This does seem like a bug.

---

_Label `bug` added by @ntBre on 2026-01-15 15:44_

---

_Label `rule` added by @ntBre on 2026-01-15 15:44_

---

_Comment by @leandrobbraga on 2026-01-20 22:43_

I have a fix in https://github.com/astral-sh/ruff/pull/22677

---
