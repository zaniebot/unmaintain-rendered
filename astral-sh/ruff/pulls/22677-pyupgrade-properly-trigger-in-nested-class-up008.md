```yaml
number: 22677
title: "[`pyupgrade`] properly trigger in nested class (`UP008`)"
type: pull_request
state: open
author: leandrobbraga
labels: []
assignees: []
base: main
head: bugfix/up008-inner-class
created_at: 2026-01-18T13:05:36Z
updated_at: 2026-01-18T13:06:49Z
url: https://github.com/astral-sh/ruff/pull/22677
synced_at: 2026-01-18T13:20:54Z
```

# [`pyupgrade`] properly trigger in nested class (`UP008`)

---

_@leandrobbraga_

I also tested it in multi-level nested classes and it worked as well.

```console
echo 'class Base:
      def __init__(self, foo):
          self.foo = foo


  class Outer(Base):
      def __init__(self, foo):
          super().__init__(foo)  # Should not trigger UP008

      class Inner(Base):
          def __init__(self, foo):
              super().__init__(foo)  # Should not trigger UP008

      class InnerInner(Base):
          def __init__(self, foo):
              super(Outer.Inner.InnerInner, self).__init__(foo)  # Should trigger UP008

  ' | cargo run -p ruff -- check --select UP008 -
UP008 Use `super()` instead of `super(__class__, self)`
  --> -:16:18
   |
14 |     class InnerInner(Base):
15 |         def __init__(self, foo):
16 |             super(Outer.Inner.InnerInner, self).__init__(foo)  # Should trigger UP008
   |                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
help: Remove `super()` parameters

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

Closes #22597 

---

_Renamed from "[`pyupgrade`] properly trigger UP008 in nested class" to "[`pyupgrade`] properly trigger in nested class (`UP008`)" by @leandrobbraga on 2026-01-18 13:06_

---
