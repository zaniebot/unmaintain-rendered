```yaml
number: 6466
title: "type-comparison (E721) doesn't work on classes with a method called `type`"
type: issue
state: closed
author: DetachHead
labels:
  - bug
assignees: []
created_at: 2023-08-10T01:11:55Z
updated_at: 2023-08-10T14:20:11Z
url: https://github.com/astral-sh/ruff/issues/6466
synced_at: 2026-01-12T15:54:46Z
```

# type-comparison (E721) doesn't work on classes with a method called `type`

---

_@DetachHead_

```py
class A:
    def asdf(self, value: str | None):
        if type(value) is str:  # error
            ...


class Foo:
    def type(self):
        pass

    def asdf(self, value: str | None):
        if type(value) is str:  # no error
            ...
```
since `type` does not refer to the shadowed method (to do that you would need to call `self.type` instead of `type`), it should not prevent the error

---

_Label `bug` added by @charliermarsh on 2023-08-10 02:23_

---

_Closed by @charliermarsh on 2023-08-10 14:20_

---
