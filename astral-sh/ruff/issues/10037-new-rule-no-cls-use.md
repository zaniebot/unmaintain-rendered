```yaml
number: 10037
title: "new rule - `no-cls-use`"
type: issue
state: open
author: DetachHead
labels:
  - rule
assignees: []
created_at: 2024-02-19T04:28:44Z
updated_at: 2024-07-26T01:51:32Z
url: https://github.com/astral-sh/ruff/issues/10037
synced_at: 2026-01-12T15:54:49Z
```

# new rule - `no-cls-use`

---

_@DetachHead_

`no-self-use` (`PLR6301`) identifies methods that do not use the `self` argument:
```py
class Foo:
    def foo(self): # error: Method `foo` could be a function, class method, or static method
        print(1)
```

but if you turn it into a class method, there's no errror, even though it would be better off as either a function or a static method:
```py
class Foo:
    @classmethod
    def foo(cls): # no error, even though cls is never used
        print(1)
```

---

_Comment by @dhruvmanila on 2024-02-21 08:03_

Thanks! I think the documentation is incorrect. We should remove the mention of "class method".

---

_Comment by @DetachHead on 2024-02-21 10:57_

in some cases it is correct to turn it into a class method. for example:
```py
class Foo:
    @staticmethod
    def bar(): ...

    def foo(self): # error: Method `foo` could be a function, class method, or static method
        print(Foo.bar())
```
in that case, the correct solution is to turn `foo` into a class method:
```py
class Foo:
    @staticmethod
    def bar(): ...

    @classmethod
    def foo(cls): # error: Method `foo` could be a function, class method, or static method
        print(cls.bar())
```

---

_Label `rule` added by @charliermarsh on 2024-07-26 01:51_

---
