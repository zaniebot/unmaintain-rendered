---
number: 6934
title: "new rule - useless `super()`"
type: issue
state: closed
author: DetachHead
labels:
  - rule
assignees: []
created_at: 2023-08-28T08:23:27Z
updated_at: 2024-02-28T06:29:45Z
url: https://github.com/astral-sh/ruff/issues/6934
synced_at: 2026-01-10T01:22:46Z
---

# new rule - useless `super()`

---

_Issue opened by @DetachHead on 2023-08-28 08:23_

```py
class Foo:
    def foo(self):
        ...


class Bar(Foo):
    def bar(self):
        super().foo() # error: use `self.foo()` instead
```

---

_Comment by @charliermarsh on 2023-08-28 14:34_

We won't be able to enforce this reliably, only for classes defined within the same module, so I'm going to close for now to keep the issue tracker actionable. We can revisit when we support multi-file analysis.

---

_Closed by @charliermarsh on 2023-08-28 14:34_

---

_Label `rule` added by @charliermarsh on 2023-08-28 14:34_

---

_Comment by @DetachHead on 2023-08-29 00:55_

@charliermarsh i don't think this rule would require multi file analysis. all it needs to do is check whether the method exists on the current subtype, and if it doesn't, assume the `super()` call is not necessary. it could either only exist on the supertype or doesn't exist at all. either way the `super()` isn't needed, so it doesn't need to look at the supertype at all

---

_Reopened by @charliermarsh on 2023-08-29 01:06_

---

_Comment by @charliermarsh on 2023-08-29 01:07_

That makes sense. What are the use-cases, though, for calling `super` on a method that isn't the current method? Should that itself be a lint error?

---

_Comment by @DetachHead on 2023-08-29 02:34_

> What are the use-cases, though

in my use case i was looking at old code after enabling some new rules and noticed a bunch of methods that were triggering `R6301` (`no-self-use`) because they were calling `super` but not `self`. i've raised #6961

> for calling `super` on a method that isn't the current method? Should that itself be a lint error?

possibly. i can't remember a time where i've needed to call `super` for a different method so maybe a rule for that would be useful too

---

_Comment by @153957 on 2023-08-31 07:25_

It *does* matter if the class using the super is subclassed and it overwrites foo.
```
class Foo:
    def foo(self):
        print('foo')


class Bar(Foo):
    def bar(self):
        super().foo()  # FooBar().bar() still calls foo on Foo instead of on FooBar


class FooBar(Bar):
    def foo(self):
        print('foobar')
```

So you would need the multifile analysis to check if any other class depends on it.

---

_Comment by @DetachHead on 2023-08-31 12:35_

good point, maybe the rule can initially only look at classes decorated with `@final` until multi-file analysis is possible

---

_Comment by @DetachHead on 2024-02-28 06:29_

pylint has a rule for this: [useless-parent-delegation](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/useless-parent-delegation.html)

so closing this in favor of #970 

---

_Closed by @DetachHead on 2024-02-28 06:29_

---
