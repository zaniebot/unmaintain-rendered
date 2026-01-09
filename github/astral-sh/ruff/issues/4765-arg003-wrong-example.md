---
number: 4765
title: "ARG003: Wrong example"
type: issue
state: closed
author: saippuakauppias
labels:
  - documentation
assignees: []
created_at: 2023-05-31T21:01:52Z
updated_at: 2023-06-01T02:24:00Z
url: https://github.com/astral-sh/ruff/issues/4765
synced_at: 2026-01-07T13:12:14-06:00
---

# ARG003: Wrong example

---

_Issue opened by @saippuakauppias on 2023-05-31 21:01_

The [rule page](https://beta.ruff.rs/docs/rules/unused-class-method-argument/) has an example of correct usage, but it itself contains an error:

```
In [34]: class MyClass:
    ...:     @classmethod
    ...:     def my_method(self, arg1):
    ...:         print(arg1)
    ...:
    ...:     def other_method(self):
    ...:         self.my_method("foo", "bar")
    ...:

In [35]: MyClass().other_method()
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
Cell In[35], line 1
----> 1 MyClass().other_method()

Cell In[34], line 7, in MyClass.other_method(self)
      6 def other_method(self):
----> 7     self.my_method("foo", "bar")

TypeError: MyClass.my_method() takes 2 positional arguments but 3 were given
```

---

_Comment by @charliermarsh on 2023-05-31 21:02_

Thanks, that's strange.

---

_Label `documentation` added by @charliermarsh on 2023-05-31 21:02_

---

_Comment by @saippuakauppias on 2023-05-31 21:03_

The neighboring rule contains the same error: https://beta.ruff.rs/docs/rules/unused-static-method-argument/

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-01 02:11_

---

_Referenced in [astral-sh/ruff#4771](../../astral-sh/ruff/pulls/4771.md) on 2023-06-01 02:14_

---

_Closed by @charliermarsh on 2023-06-01 02:24_

---
